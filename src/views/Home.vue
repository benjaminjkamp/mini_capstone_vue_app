<template>
  <div class="home container">
    <h1>{{ message }}</h1>
    
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    <div>
      <input type="text" v-model="nameFilter" list="names">
    </div>

    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>

    <button v-on:click="selectSort('name')">Name
      <span v-if="(sortAttribute === 'name' && sortAscending === 1)">^</span>
      <span v-if="(sortAttribute === 'name' && sortAscending === -1)">v</span>
    </button>
    <button v-on:click="selectSort('price')">Price
      <span v-if="(sortAttribute === 'price' && sortAscending === 1)">^</span>
      <span v-if="(sortAttribute === 'price' && sortAscending === -1)">v</span>
    </button>
    <br>
    <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
      <div v-bind:key="product.id" v-for="product in orderBy(filterBy(products, nameFilter, 'name'), sortAttribute, sortAscending)">
        <h2> {{ product.name }} </h2>
        <h3> <router-link :to="'/products/' + product.id"><img :src="product.image_url"> </router-link></h3>
        <router-link :to="'/products/' + product.id">Show Details</router-link>
        
        <br>
       
        <div v-if="currentProduct === product">
          <h3> Description: {{ product.description }} </h3>
          <h3> Price: {{ product.price }}</h3>
          <div>
            <button class="btn btn-warning">
              <router-link to="/products/edit">Edit Product</router-link>
            </button>
            <button v-on:click="destroyProduct(product)">Destroy This Product</button>

          </div>
          
        </div>
        <br>
      </div>
    </transition-group>
  </div>
</template>

<style>
</style>

<script>
import Vue2Filters from "vue2-filters";
import axios from "axios";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Products",
      products: [],
      currentProduct: "",
      newProductName: "",
      newProductDesc: "",
      newProductPrice: "",
      newProductImage: "",
      errors: [],
      nameFilter: "",
      sortAttribute: 'name',
      sortAscending: 1

    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
          this.products = response.data;
        });
  },


  methods: {
   
    addProduct: function(){
      var params = {
        name: this.newProductName,
        description: this.newProductDesc,
        price: this.newProductPrice,
        image_url: this.newProductImage
      };

      axios.post("/api/products", params).then(response => {
        console.log("Success", response.data);
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductDesc = "";
        this.newProductPrice = "";
        this.newProductImage = "";
      }).catch(error => {
        this.errors = error.response.data;
        console.log(error.response.data);
      })
    },

    updateProduct: function(product){
      var params = {
        name: product.name,
        description: product.description,
        price: product.price,
        image_url: product.image_url
      };

      axios.patch("/api/products/" + product.id, params).then(response => {
        console.log("updated", response.data);
        product = response.data;
      })
    },

    destroyProduct: function(product){
      axios.delete("/api/products/" + product.id).then(response => {
        console.log("Product Destroyed", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });

      

    },
    selectSort(attribute){
      if (this.sortAttribute === attribute) {
        this.sortAscending = this.sortAscending * -1;
      } else {
        this.sortAscending = 1;
      }
      this.sortAttribute = attribute
    }
  }
};


</script>