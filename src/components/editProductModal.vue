<template>  
<transition name="modal">
    <div class="modal-mask">
        <div class="modal-wrapper">
            <div class="modal-container">

              <div class="modal-header">
                <slot name="header">
                  <h3>Edit Product</h3>
                </slot>
              </div>

              <div class="modal-body">
                <slot name="body">
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <span class="input-group-text" id="productName">Product name: </span>
                        </div>
                        <input type="text" class="form-control col-8" v-model="productCopy.productName" aria-label="Product name" aria-describedby="productName">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <label class="input-group-text" for="productSupplier">Supplier: </label>
                        </div>
                        <select class="custom-select col-8" id="productSupplier" v-model="productCopy.supplierId">
                            <option v-for="s in suppliers" :key="s.supplierId" :value="s.supplierId">{{s.companyName}}</option>
                        </select>
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <label class="input-group-text" for="productCategory">Category: </label>
                        </div>
                        <select class="custom-select col-8" id="productCategory" v-model="productCopy.categoryId">
                            <option v-for="c in categories" :key="c.categoryId" :value="c.categoryId">{{c.categoryName}}</option>
                        </select>
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <span class="input-group-text" id="productQuantityPerUnit">Quantity per unit: </span>
                        </div>
                        <input type="text" class="form-control col-8" v-model="productCopy.quantityPerUnit" aria-label="Quantity per unit" aria-describedby="productQuantityPerUnit">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <span class="input-group-text" id="productUnitPrice">Price: </span>
                        </div>
                        <input type="number" class="form-control col-8" v-model="productCopy.unitPrice" aria-label="Price" aria-describedby="productUnitPrice">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend col-4">
                            <span class="input-group-text" id="productUnitsInStock">Units in stock: </span>
                        </div>
                        <input type="text" class="form-control col-8" v-model="productCopy.unitsInStock" aria-label="Units in stock" aria-describedby="productUnitsInStock">
                    </div>
                </slot>
              </div>

              <div class="modal-footer">
                <slot name="footer">
                  <button class="modal-default-button btn btn-danger"  @click="$emit('close')">
                    Cancel
                  </button>
                  <button class="modal-default-button btn btn-success" @click="EditProduct(productCopy.productId)">
                    <i class="fa fa-save"></i> Save
                  </button>
                </slot>
              </div>
            </div>
        </div>
    </div>
</transition>
</template>

<script>
import axios from 'axios'

export default {
    name:'editProductModal',
    props:{
        product:Object
    },
    data(){
        return {
            productCopy:{},
            products:[],
            suppliers:[],
            categories:[],
        }
    },
    methods:{
        EditProduct(id){
            axios
            .put(`http://pabp.viser.edu.rs:8000/api/Products/${id}`,this.productCopy)
            .then(response=>{
                console.log(response)
                this.$emit('close');
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
                console.log('Error: ' + err)
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
        close() {
            this.$emit('close');
        },
    },
    mounted(){
        this.productCopy=this.product
        this.GetSuppliers()
        this.GetCategories()
    }
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}
.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
.modal-container {
  width: 40%;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}
.modal-body {
  margin: 20px 0;
}
.modal-default-button {
  float: right;
}
.modal-enter {
  opacity: 0;
}
.modal-leave-active {
  opacity: 0;
}
.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>