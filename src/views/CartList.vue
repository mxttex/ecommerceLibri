<script>
    export default{
        props:["cartList"],
        data(){
            return{
                books: this.cartList,
            }
        },
        emits:["remove-from-cart"]
        }
</script>

<template>
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
                    <td><div class="d-flex justify-content-center"><button class="btn btn-primary" @click="$emit('remove-from-cart', books, false)">Remove from Cart</button></div></td>
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