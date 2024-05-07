<script>
  import { RouterLink, RouterView } from 'vue-router'
  import SiteNavbar from './components/SiteNavbar.vue';

  export default
  {
    data(){
      return{
        catalogEndpoint : "http://localhost:3000/books/",
        cartEndpoint : "http://localhost:3000/cart/",
        books: [],
        allBooks: [],
        catalogMode : true,
        filter : ""
      }
    },
    async created(){
      this.allBooks = await this.fetchBooks();
      this.books = this.allBooks;
    },
    methods:{
      async fetchBooks(){
        let endpoint;
        endpoint = this.catalogMode ? this.catalogEndpoint : this.cartEndpoint;

        const list = await fetch(endpoint);
        const data = list.json();

        return data;
      },
      reloadFilter(filter){
        let filteredBooks = [];

        if(filter === ""){
          filteredBooks = this.allBooks;
        }
        else{
          filteredBooks = this.allBooks.filter(
            book => { return book.title.includes(filter)}
          )
        }

        this.books = filteredBooks;
      },
      async changeMode(mode){
        this.catalogMode = mode;
        console.log(mode)
        this.allBooks = await this.fetchBooks();
        this.book = this.allBooks;

        console.log(this.allBooks)
      }

    },
    components:{SiteNavbar}
  }
</script>

<template>
  <SiteNavbar @filter-changed="reloadFilter" @change-view="changeMode"/>  
  <RouterView :catalogList="this.books" :cartList="this.books"></RouterView>
</template>

<style scoped>
  nav{
    align-items: top;
    align-content: top;
  }
</style>
