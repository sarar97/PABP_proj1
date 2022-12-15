<template>
    <tr> 
        <td class="col-3">
            {{productCopy.productName}}
        </td>
        <td class="col-3">
            {{supp}}
        </td>
        <td class="col-1">
            {{productCopy.unitPrice}}
        </td>
        <td class="col-1">
            {{productCopy.unitsInStock}}
        </td>
        <td class="col-1">
            {{productCopy.unitPrice * productCopy.unitsInStock}}
        </td>
        <td class="col-3">
            <button class="btn btn-secondary m-1" @click="showModal = true"><i class="far fa-edit"></i></button> 
            <button class="btn btn-secondary m-1" @click="ProductDelete(productCopy.productId)" ><i class="far fa-trash-alt"></i></button>
            <button class="btn btn-secondary m-1" type="button">
                <router-link :to="{ name: 'productDetails', params: { productId: productCopy.productId }}">Details</router-link>
            </button>
        </td>  
    </tr>

    <editProductModal v-if="showModal" @close="showModal = false" :product="product"/>

</template>

<script >
import axios from 'axios'
import editProductModal from './editProductModal.vue'

export default {
    name:'productRow',
    props:{
        product:Object
    },
    emits:[
        'productDeleteEvent',
    ],
    components: {
      editProductModal,
    },
    data(){
        return{
            supp:"",
            productCopy:[],
            prikazDetalja:false,
            showModal: false
        }
    },
    methods:{
        GetSupplier(id){
            axios
            .get(`http://pabp.viser.edu.rs:8000/api/Suppliers/${id}`)
            .then(response=>{
                this.supp = response.data.companyName
            })
            .catch(err=>{
                console.log(err)
            })
        },
        ProductDelete(id){
            axios
            .delete(`http://pabp.viser.edu.rs:8000/api/Products/${id}`)
            .then(
                this.$emit('productDeleteEvent', id)
            )
            .catch(err=>{
                console.log(err)
            })
        }
    },
    mounted(){
        this.productCopy=this.product
        if(this.product.supplierId != null){
            this.GetSupplier(this.product.supplierId)
        }else{
            this.supp = "Supplier unknown"
        }
    }
}
</script>

<style>

</style>