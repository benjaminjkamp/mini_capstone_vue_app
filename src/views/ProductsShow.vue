<template>
  <div class="products-show container">
    <h1>{{ message }}</h1>
    
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    <br><br>
    <h2>{{ product.name }}</h2>
    <img :src="product.image_url"><br><br>
    <h4>Description: {{ product.description }} </h4>
    <h4>Price: {{ product.price }} </h4>
    <br><br>
    <!-- <button class="btn btn-warning"> -->
      <router-link class="btn btn-warning" :to="'/products/' + product.id + '/edit'" >Edit Product</router-link>
    <!-- </button> -->
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Product",
      product: {},
      currentProduct: "",
      newProductName: "",
      newProductDesc: "",
      newProductPrice: "",
      newProductImage: "",
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
          this.product = response.data;
        });
  },


  methods: {
   
     
  }
};


</script>