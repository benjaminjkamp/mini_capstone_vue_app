<template>
  <div class="home container">
    <h1>{{ message }}</h1>
    
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    
    <form v-on:submit.prevent="submit()">
      <div class="form-row">
        <div class="form-group col-md-6">
          <label for="name">Name</label>
          <input type="text" class="form-control" id="name" placeholder="Product Name" v-model="product.name">
        </div>
        <div class="form-group col-md-6">
          <label for="price">Price</label>
          <input type="number" step="any" class="form-control" id="price" placeholder="$" v-model="product.price">
        </div>
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description" placeholder="Describe Product..." v-model="product.description">
      </div>
      <div class="form-group">
        <label for="image">Image</label>
        <input type="text" class="form-control" id="image" placeholder="http://..." v-model="product.image_url">
      </div>
      <button type="submit" class="btn btn-success">Update Product</button>
      <!-- <button type="submit" class="btn btn-primary">Delete Product</button> -->      
      
    </form>
    <button type="" class="btn btn-danger" v-on:click="destroyProduct()">Delete Product</button>
    
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Edit Product",
      product: {},
      currentProduct: "",
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
          this.product = response.data;
        });
  },


  methods: {
    submit: function(){
      var params = {
        name: this.product.name,
        description: this.product.description,
        price: this.product.price,
        image_url: this.product.image_url
      };

      axios.patch("/api/products/" + this.product.id, params).then(response => {
        console.log("updated", response.data);
        this.product = response.data;
        this.$router.push("/products/" + this.product.id);
      }).catch(error => {
        console.log(error.response.data);
        this.errors = error.response.data;
      })
    },

    destroyProduct: function(product){
      axios.delete("/api/products/" + this.product.id).then(response => {
        console.log("Product Destroyed", response.data);
        this.$router.push("/")
      }).catch(error => {
        console.log(error.response.data);
        this.errors = error.response.data;
      });

    }
  }
};


</script>