<script>
    import SiteNavbar from './SiteNavbar.vue';
    export default{
        data(){
            return{
                endpoint : "http://localhost:3000/cart/",
                books : [],
                allBooks: []
            }
        },
        components:{SiteNavbar},
        methods:{
            async loadFilteredBooks(filter){
                let filteredBooks = [];
                if(filter === ""){
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
                const list = await fetch(this.endpoint);
                const data = list.json()
                
                return data
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
    <div class="container-fluid">
        <SiteNavbar @filter-changed="reloadBooks" />
        <div class="table">
            <thead>
                <tr>
                    <td class="col-10">BOOK</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="cartBook in this.books" :key="cartBook.id">
                <td ckass="col-3"><img :src="cartBook.coverUrl"></td>
                <td>{{ cartBook.title }}</td>
                <td>{{ cartBook.author }}</td>
                <td>{{ cartBook.category }}</td>
                <td><button class="btn btn-primary">Remove from Cart</button></td>
                </tr>
            </tbody>
        </div>
    </div>
</template>

<style>
    SiteNavbar{
        width: 100%;
    }
    table{
        border: 1px solid black;
        width: 80%;
        
    }
    img{
        width: 80px;
    }
    
    
</style>