<template>
    <button type="button" class="btn btn-secondary btn-lg btn-block homeBtn mb-2"><router-link to="/">Back to home page</router-link></button>

    
</template>

<script>
import axios from 'axios'
import productRow from '../components/productRow.vue'
import addProduct from '../components/addProduct.vue'

export default {
    name:'productTable',
    props:{
        categoryId:String
    },
    components:{
        productRow,
        addProduct
    },
    data(){
        return {
            products:[],
            filteredProducts:[],
            suppliers:[],
        }
    },
    methods:{
        GetProduct(categoryId){
            axios
            .get(`http://pabp.viser.edu.rs:8000/api/Products/${categoryId}`)
            .then(response=>{
                this.product= response.data
                console.log(this.product)
            })
            .catch(err=>{
                console.log(err)
            })
        },
        GetSuppliers(){
            axios
            .get(`http://pabp.viser.edu.rs:8000/api/Suppliers`)
            .then(response=>{
                this.suppliers = response.data
            })
            .catch(err=>{
                console.log(err)
                // alert(`Greska:${err}`)
            })
        } 
    },
    mounted(){
        this.GetProduct(productId)
        this.GetSuppliers()
    }
}
</script>

<style>
.homeBtn a{
    color: white;
    text-decoration: none;
}
</style>