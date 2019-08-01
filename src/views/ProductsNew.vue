<template>
  <div class="container new">
    <h1>{{ message }}</h1>
    <h2>New property</h2>
    Name:
    <input v-model="name" type="text" />
    Price:
    <input v-model="price" type="text" />
    Description:
    <input v-model="description" type="text" />
    Image URL:
    <input v-model="image_url" type="text" />
    Supplier:
    <input v-model="supplier" type="text" />
    <button v-on:click="createProperty">Create property</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      inputProduct: [],
      more_info: {}
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    createProperty: function() {
      console.log("Create new property");
      var params = {
        name: this.name,
        price: this.price,
        description: this.description,
        image_url: this.imageUrl,
        supplier: this.supplier
      };
      axios.post("/api/products", params).then(response => {
        console.log("Success", response.data);
        this.products.push(response.data);
        this.name = "";
        this.price = "";
        this.description = "";
        this.imageUrl = "";
        this.supplier = "";
      });
    }
  }
};
</script>
