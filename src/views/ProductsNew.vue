<template>
  <div class="products-new container">
    <h1>{{ message }}</h1>
    <form v-on:submit.prevent="submit()">
      <div class="form-row">
        <div class="form-group col-md-6">
          <label for="name">Name</label>
          <input type="text" class="form-control" id="name" placeholder="Product Name" v-model="newProductName">
        </div>
        <div class="form-group col-md-6">
          <label for="price">Price</label>
          <input type="number" class="form-control" id="price" placeholder="$" v-model="newProductPrice">
        </div>
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description" placeholder="Describe Product..." v-model="newProductDesc">
      </div>
      <div class="form-group">
        <label for="image">Image</label>
        <input type="text" class="form-control" id="image" placeholder="http://..." v-model="newProductImage">
      </div>
      <button type="submit" class="btn btn-primary">Add Product</button>
    </form>
    
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
    
    
    
    
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "New Product",
      newProductName: "",
      newProductDesc: "",
      newProductPrice: "",
      newProductImage: "",
      errors: []
    };
  },
  created: function() {
  },


  methods: {
   
    submit: function(){
      var params = {
        name: this.newProductName,
        description: this.newProductDesc,
        price: this.newProductPrice,
        image_url: this.newProductImage
      };

      axios.post("/api/products", params).then(response => {
        console.log("Success", response.data);
        this.$router.push('/');
      }).catch(error => {
        this.errors = error.response.data;
        console.log(error.response.data);
      })
    }
  }
};


</script>