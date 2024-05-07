<script>
    export default{
        props:["catalogList"],
        
        emits:["add-to-cart"]
        }
</script>

<template>
    <div class="container-fluid d-flex justify-content-center">
        <div class="row justify-content-left" style="display: flex;">
            <div class="col-md-3" v-for="book in this.catalogList" :key="book.id" style="flex: 1;">
                <div class="card m-1 h-100">
                    <img :src="book.coverUrl" class="card-img-top" :alt="book.title" >
                    <div class="card-body d-flex flex-column">
                        <h5 class="card-title">{{ book.title }}, {{ book.author }}</h5>
                        <p class="card-text">Category: {{ book.category }} || First Publish Year: {{ book.first_publish_year }}</p>
                        <a class="btn btn-primary mt-auto" @click="$emit('add-to-cart', book, true)">Add to Cart</a>
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
        max-width: 280px;
        align-items: center;
    }
    img{
        height: 400px;
        width: 280px;
    }
</style>