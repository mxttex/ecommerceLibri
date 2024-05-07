<script>
  import { RouterLink, RouterView } from 'vue-router'
  import SiteNavbar from './components/SiteNavbar.vue';

  export default
  {
    data(){
      return{
        catalogEndpoint : "http://localhost:3000/books/",
        cartEndpoint : "http://localhost:3000/cart/",
        catalogBooks: [],
        cartBooks: [],
        filter : ""
      }
    },
    async mounted(){
      this.catalogBooks = await this.fetchBooks(true);
      this.cartBooks = await this.fetchBooks(false)
    },
    async created(){
      this.catalogBooks = await this.fetchBooks(true);
      this.cartBooks = await this.fetchBooks(false)
    },
    methods:{
      async fetchBooks(mode){
        let endpoint;
        endpoint = mode ? this.catalogEndpoint : this.cartEndpoint;
        const list = await fetch(endpoint);
        const data = list.json();

        return data;
      },
      async reloadFilter(filter){
        this.filter = filter;
        if(this.filter === ""){
          this.cartBooks = await this.fetchBooks(false);
          this.catalogBooks = await this.fetchBooks(true);
        }
        else{
          this.catalogBooks = this.catalogBooks.filter(
            book => { return book.title.includes(filter)}
          )
          this.cartBooks = this.cartBooks.filter(
            book => { return book.title.includes(filter)}
          )
        }

        
      },
      async changeMode(mode){
        
        if(mode){
          this.catalogBooks = await this.fetchBooks(true);
        }
        else{
          this.cartBooks = await this.fetchBooks(false);
        }
      },
      async addOrRemoveFromCart(book, mode){
        
        let firstEndpoint, secondEndpoint;
        if(mode){
          //se è true vuol dire che stiamo aggiungendo un libro al carrello
          firstEndpoint = this.cartEndpoint;
          secondEndpoint = this.catalogEndpoint + book.id;
        }
        else{
          firstEndpoint = this.catalogEndpoint;
          secondEndpoint = this.cartEndpoint + book.id;
        }

        await fetch(firstEndpoint,
          {
            method : "POST",
            headers:{
              'Content-type':'application/json'
            },
            body :  JSON.stringify(book)
          }
        )

        await fetch(secondEndpoint,
          {
            method : "DELETE",
            headers : {
              'Content-type' : 'application/json'
            }
          }
        )

        this.catalogBooks = await this.fetchBooks(true)
        this.cartBooks = await this.fetchBooks(false)

      },
      async checkout(){
        // for(let i = 0; i< this.cartBooks.length; i++){
        //   await this.addOrRemoveFromCart(this.cartBooks[i], false);
        // }
        this.cartBooks.forEach(
          async book =>{
            await this.addOrRemoveFromCart(book, false)
          }
        )

        this.cartBooks = await this.fetchBooks(false);
      }

    },
    components:{SiteNavbar}
  }
</script>

<template>
  <SiteNavbar @filter-changed="reloadFilter" @change-view="changeMode"/>  
  <RouterView v :catalogList="this.catalogBooks" :cartList="this.cartBooks" @add-to-cart="addOrRemoveFromCart" @remove-from-cart="addOrRemoveFromCart" @checkout="checkout"></RouterView>
</template>

<style scoped>
  nav{
    align-items: top;
    align-content: top;
  }
</style>
