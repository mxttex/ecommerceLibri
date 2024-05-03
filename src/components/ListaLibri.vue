<script>
import SiteNavbar from './SiteNavbar.vue';

    export default{
        data(){
            return{
                endpoint : "http://localhost:3000/books/",
                books: [],
                allBooks: []
            }
        },
        async created(){
            this.books = await this.fetchBooks("");
            this.allBooks = this.books;
        },
        methods:{
            async fetchBooks(){
            const list = await fetch(this.endpoint);
            const data = list.json()
            
            return data
            },   
            async FilterBook(filter){
                let filteredBooks = [];
                if(filter === ""){
                    filteredBooks = this.allBooks;
                }
                else{
                    filteredBooks = this.allBooks.filter(
                        book => {
                            return book.title.includes(filter)
                        }
                    )
                }
                return filteredBooks
            },
            async reloadBooks(filter){
                this.books = await this.FilterBook(filter);
            }
        },
        components:{SiteNavbar}
    }
    
</script>

<template>
    <SiteNavbar @filter-changed="reloadBooks"/>
    <div class="container-fluid">
        <div class="row justify-content-left" style="display: flex;">
            <div class="col-md-3" v-for="book in this.books" :key="book.id" style="flex: 1;">
                <div class="card m-1 h-100">
                    <img :src="book.coverUrl" class="card-img-top" :alt="book.title">
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title">{{ book.title }}, {{ book.author }}</h5>
                        <p class="card-text">Category: {{ book.category }} || First Publish Year: {{ book.first_publish_year }}</p>
                        <a class="btn btn-primary mt-auto">Add to Cart</a>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>
<style scoped>
    .card {
        max-height:800px; /* Imposta l'altezza massima desiderata */
        overflow: hidden; /* Impedisce il contenuto di sforare l'altezza massima */
        max-width: 300px;
    }
    img{
        height: 400px;
        width: 280px;
    }
</style>