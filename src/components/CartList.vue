<script>
    import SiteNavbar from './SiteNavbar.vue';
    export default{
        data(){
            return{
                endpoint : "http://localhost:3000/cart/",
                books : []
            }
        },
        components:{SiteNavbar},
        methods:{
            async reloadBooks(){

            }
        },
        async created(){
            cartBooks = await this.reloadBooks();
        }
    }       
    

</script>

<template>
    <div class="container-fluid">
        <SiteNavbar @filter-changed="reloadBooks"></SiteNavbar>
        <div>QUI CI SARA' LA TABELLA DEL CARRELLO</div>
    </div>
</template>