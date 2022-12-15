<template>
  <h3>Select a category</h3>
  <div class="row">
    <div v-for="c in categories" :key="c.categoryId" :value="c.categoryId" class="col-2">
      <div class="card">
        <img class="card-img-top" v-bind:src="'data:image/png;base64,'+ c.picture" :alt="c.categoryName">
        <!-- <img class="card-img-top" src="../assets/categoryIcon.png" alt=""> -->
        <div class="card-body card-width">
          <router-link :to="{ name: 'category', params: { categoryId: c.categoryId }}"><h5 class="card-title">{{c.categoryName}}</h5></router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'HomeView',
  components: {
  },
  data(){
    return {
      categories:[],
    }
  },
  methods:{
    GetCategories(){
      axios 
        .get(`http://pabp.viser.edu.rs:8000/api/Categories`)
        .then(response => {
          this.categories = response.data
        })
        .catch(error => {
          console.log(`Error: ${error}`)
        })
    }
    },
    mounted(){
        this.GetCategories()
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: auto;
  width: 90%;
  padding: 10px;
}
.card-width {
  width: 100%;
  margin: 0;
}
.card{
  margin-top: 15px;
}
.card:hover{
  background-color: #f3f1f1;
}
</style>