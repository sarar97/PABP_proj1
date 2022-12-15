<template>  
<transition name="modal">
    <div class="modal-mask">
        <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-header">
                <slot name="header">
                  <h3>Add Product</h3>
                  <button type="button" class="close" @click="close()">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </slot>
              </div>

              <div class="modal-body">
                <slot name="body">
                    <form class="offset-2 col-8">
                        <div class="form-group row">
                            <label for="imeFormControlInput" class="col-4 col-form-label">Product name : </label>
                            <div class="col-8">
                                <input type="text" v-model="newProduct.productName" class="form-control" id="imeFormControlInput">
                            </div>
                        </div>
                        <div class="row">
                            <label for="supplierFormControlInput" class="col-4 col-form-label">Supplier: </label>
                            <div class="col-8">
                                <select v-model="newProduct.supplierId" class="custom-select col-12" id="supplierFormControlInput">
                                    <option v-for="s in suppliers" :key="s.supplierId" :value="s.supplierId">
                                        {{s.companyName}}
                                    </option>
                                </select>
                            </div>
                        </div>
                        <div class="row">
                            <label for="categoryFormControlInput" class="col-4 col-form-label">Category: </label>
                            <div class="col-8">
                                <input type="text" v-model="category.categoryName" disabled>
                            </div>
                        </div>
                        <!-- <div class="row">
                            <label for="categoryFormControlInput" class="col-4 col-form-label">Category: </label>
                            <div class="col-8">
                                <select v-model="newProduct.categoryId" class="custom-select col-12" id="categoryFormControlInput">
                                    <option v-for="c in categories" :key="c.categoryId" :value="c.categoryId">
                                        {{c.categoryName}}
                                    </option>
                                </select>
                                
                            </div>
                        </div> -->
                        <div class="row">
                            <label for="priceFormControlInput" class="col-4 col-form-label">Price: </label>
                            <div class="col-8">
                                <input type="number" min="1" v-model="newProduct.unitPrice" class="form-control" id="priceFormControlInput">
                            </div>
                        </div>
                        <div class="row">
                            <label for="quantityFormControlInput" class="col-4 col-form-label">Units in stock: </label>
                            <div class="col-8">
                                <input type="number" min="0" v-model="newProduct.unitsInStock" class="form-control" id="quantityFormControlInput">
                            </div>
                        </div>
                        <div class="row">
                            <label for="discontinuedCheckbox" class="col-4 form-check-label">Discontinued: </label>
                            <div class="col-8">
                                <input type="checkbox" v-model="newProduct.discontinued" class="form-check-input" id="discontinuedCheckbox">
                            </div>
                        </div>
                        <button @click="AddProduct()" type="submit" class="btn btn-success">Add product</button>
                    </form>
                </slot>
              </div>

              <!-- <div class="modal-footer">
                <slot name="footer">
                  <button @click="AddProduct()" class="modal-default-button btn btn-success">Add product</button>
                </slot>
              </div> -->
            </div>
        </div>
    </div>
</transition>
</template>

<script>
import axios from 'axios'

export default {
    name:'addProduct',
    props:{
        categoryId:String
    },
    emits:[
        'productAddEvent',
        'closeProductAdd'
    ],
    data(){
        return {
            products:[],
            suppliers:[],
            newProduct:{},
            categories:[],
            category:{},
            productValid:false,
        }
    },
    methods:{
        AddProduct(){
            axios
            .post(`http://pabp.viser.edu.rs:8000/api/Products`,this.newProduct)
            .then(response=>{
                this.newProduct.productId = response.data.productId
                this.products.push(this.newProduct)
                this.newProduct = {}
                this.$emit('productAddEvent');
            })
            .catch(err=>{
                console.log('Error: ' + err)
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
            })
        },
        GetCategory(id){
            axios
            .get(`http://pabp.viser.edu.rs:8000/api/Categories/${id}`)
            .then(response=>{
                this.category = response.data
            })
            .catch(err=>{
                console.log(err)
            })
        },
        GetCategories(){
            axios 
            .get(`http://pabp.viser.edu.rs:8000/api/Categories`)
            .then(response => {
                this.categories = response.data
            })
            .catch(error => {
                console.log(`Error: ${error}`)
            })
        },
        close(){
            this.$emit('closeProductAdd')
        }
    },
    mounted(){
        this.GetSuppliers()
        this.GetCategories()
        this.GetCategory(this.categoryId)
        this.newProduct.categoryId = this.categoryId
    }
}
</script>

<style>
input, option, select {
  height: 25pt;
  box-sizing: border-box;
}
</style>