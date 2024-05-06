<template>
    <nav class="navbar bg-primary border-bottom border-bod navbar-expand-lg" style="color: white;">
        <div class="container-fluid">
            <a class="navbar-brand">I Libri del Faccettz</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarScroll" aria-controls="navbarScroll" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarScroll">
            <ul class="navbar-nav me-auto my-2 my-lg-0 navbar-nav-scroll" style="--bs-scroll-height: 100px;">
                <li class="nav-item">
                    <a class="nav-link" >
                        <router-link to="/" style="color: aliceblue;">Home</router-link>
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" >
                        <router-link to="/carrello" style="color: aliceblue;">Cart</router-link>
                    </a>
                </li>
                
            </ul>
            <form class="d-flex" role="search" @submit.prevent="onSubmit">
                <input class="form-control me-2" v-model="filter" type="search" placeholder="Book's title" aria-label="Search">
                <button class="btn btn-primary" type="submit">Search</button>
            </form>
            </div>
        </div>
    </nav>
</template>

<style scoped>
    a{
        color: white;
    }
    router-link{
        text-decoration: none;
    }
    
</style>

<script>
    export default{
        data(){
            this.filter = "";
        },
        methods:{
            onSubmit(e){
                this.$emit("filter-changed", this.filter)
            }
        }
    }
</script>