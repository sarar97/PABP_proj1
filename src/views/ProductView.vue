<template>
    <button type="button" class="btn btn-secondary btn-lg btn-block homeBtn mb-2"><router-link to="/">Back to home page</router-link></button>

    <form class="offset-3 col-6">
        <div class="form-group row">
            <div class="input-group mb-3">
                <div class="input-group-prepend col-3">
                    <label class="input-group-text" for="inputSupplierSelect">Supplier: </label>
                </div>
                <select class="custom-select col-9" id="inputSupplierSelect" v-model="filterSupplier">
                    <option :value="null" selected>Not selected</option>
                    <option v-for="s in suppliers" :key="s.supplierId" :value="s">{{s.companyName}}</option>
                </select> 
            </div>
        </div>

        <div class="form-group row">
            <div class="input-group">
                <div class="input-group-prepend col-3">
                    <label class="input-group-text" for="inputPrice">Price: </label>
                </div>
                <input ref="inputPrice" v-on:keyup="checkFilterPriceInput" type="number" min="0" class="form-control col-9" id="inputPrice" v-model="filterPrice">
            </div>
        </div>

        <button type="button" class="btn btn-primary" v-on:click="filterProducts">Filter products</button>
    </form>

    <button class="btn btn-secondary m-3" @click="showModal = true" :categoryId="categoryId">Add New Product</button> 

    <table class="table table-hover table-bordered">
        <thead class="thead-light">
            <tr>
                <th scope="col-3">Product name</th>
                <th scope="col-3">Supplier</th>
                <th scope="col-1">Price</th>
                <th scope="col-1">Quantity</th>
                <th scope="col-1">Value</th>
                <th scope="col-3"></th>
            </tr>
        </thead>
        <tbody>
            <productRow v-for="p in filteredProducts" :key="p.productId"
            :product="p" @productDeleteEvent="onProductDelete" />
        </tbody>
    </table>   

    <addProduct v-if="showModal" @closeProductAdd="handleClose()"  @productAddEvent="onProductAdd()" :categoryId="categoryId"/>
    
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
            filterSupplier: null,
            filterPrice: null,
            showModal: false,
        }
    },
    methods:{
        checkFilterPriceInput: function(e){
            let trenutnaCena = e.target.value;
            console.log(trenutnaCena);
            if(trenutnaCena == "" || trenutnaCena == undefined || trenutnaCena <= 0){
                e.target.value = null;
            }
        },
        filterProducts: function(){
            this.filteredProducts = this.products;

            if(this.filterPrice == ""){
                this.filterPrice = null;
            }

            let filteredBySupplier = this.filterSupplier == null ? this.filteredProducts : this.products.filter(x=> x.supplierId == this.filterSupplier.supplierId);
            
            let filteredByPrice = this.filterPrice == null ? filteredBySupplier : filteredBySupplier.filter(x=> x.unitPrice <= this.filterPrice);

            this.filteredProducts = filteredByPrice;

        },
        GetProducts(categoryId){
            axios
            .get(`http://pabp.viser.edu.rs:8000/api/Products`)
            .then(response=>{
                this.products= response.data.filter(p => p.categoryId == categoryId);
                this.filteredProducts = this.products;
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
        },
        onProductDelete(id){
            for(var i in this.filteredProducts){
                if(this.filteredProducts[i].productId == id){
                    this.filteredProducts.splice(i, 1)
                }
            }
            alert("Product deleted successfully")
        },
        onProductAdd(){
            this.showModal = false
        },
        handleClose(){
            this.showModal = false
            alert("handle close fired")
        }   
    },
    mounted(){
        this.GetProducts(this.categoryId)
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