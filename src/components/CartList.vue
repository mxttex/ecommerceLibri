<script>
    import SiteNavbar from './SiteNavbar.vue';
    export default{
        data(){
            return{
                cartEndpoint : "http://localhost:3000/cart/",
                homeEndpoint : "http://localhost:3000/books/",
                books : [],
                allBooks: [],
            }
        },
        components:{SiteNavbar},
        methods:{
            async loadFilteredBooks(filter){
                let filteredBooks = [];
                if(filter === " "){
                    filteredBooks = this.allBooks; 
                }
                else{
                    filteredBooks = this.allBooks.filter(
                        book => {return book.title.includes(filter)}
                    )
                }

                return filteredBooks;
            },
            async reloadBooks(filter){
                this.books = await this.loadFilteredBooks(filter);
            },
            async loadBooks(){
                const list = await fetch(this.cartEndpoint);
                const data = list.json()
                return data
            },
            async removeFromCart(book){
                await fetch(this.homeEndpoint,
                {
                    method:'POST',
                    headers:{
                    'Content-type':'application/json'
                    },
                    body: JSON.stringify(book)
                })
                await fetch(this.cartEndpoint + book.id,
                {
                    method:'DELETE',
                    headers:{
                    'Content-type':'application/json'
                    }
                })
                this.allBooks = await this.loadBooks();
                this.reloadBooks(" ");
            }
        },
        async created(){
            this.allBooks = await this.loadBooks();
            this.books = this.allBooks;
            console.log(this.allBooks)
        }
    }       
    

</script>

<template>
    <SiteNavbar @filter-changed="reloadBooks" />
    <div >
        <table class="table table-striped">
            <thead class="thead-dark">
                <tr>
                    <th scope="col">Cart Books</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="book in this.books" :key="book.id">
                    <td><img :src="book.coverUrl" style="max-width: 100px;"></td>
                    <td><div class="d-flex justify-content-left">{{ book.title }}</div></td>
                    <td><div class="d-flex justify-content-left">{{ book.author }}</div></td>
                    <td><div class="d-flex justify-content-left">{{ book.category }}</div></td>
                    <td><div class="d-flex justify-content-center"><button class="btn btn-primary" @click="removeFromCart(book)">Remove from Cart</button></div></td>
                </tr>
            </tbody>
        </table>

        <button class="btn btn-primary">Checkout</button>
    </div>

</template>

<style>
    SiteNavbar{
        width: 100%;
    }
    table{
        border: 5px solid black;
        width: 80%;
        
    }
    img{
        width: 80px;
    }
    td{
        justify-content: center;
        align-content: center;
    }
    
    
</style>