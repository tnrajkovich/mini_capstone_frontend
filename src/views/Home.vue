<template>
  <div class="home">
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
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <p>{{ product.price }}</p>
      <img v-bind:src="product.image_url" alt="" />
      <div>
        <button v-on:click="currentProduct = product">More info</button>
        <div v-if="product === currentProduct">
          <p>Name: {{ currentProduct.name }}</p>
          <p>Price: {{ currentProduct.price }}</p>
          <p>Description: {{ currentProduct.description }}</p>
        </div>
        <div>
          <h2>Edit property</h2>
          Name:
          <input v-model="product.name" type="text" />
          Price:
          <input v-model="product.price" type="text" />
          Description:
          <input v-model="product.description" type="text" />
          Image URL:
          <input v-model="product.image_url" type="text" />
          <button v-on:click="updateProduct(product)">Edit property</button>
        </div>
      </div>
      <button v-on:click="deleteProduct(product)">Delete property</button>
    </div>
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
  },
  showProperty: function(product) {
    if (this.currentProduct === inputProduct) {
      this.currentProduct = {};
    } else {
      this.currentProduct = inputProduct;
    }
  },
  updateProduct: function(product) {
    var params = {
      name: inputProduct.name,
      price: inputProduct.price,
      description: inputProduct.description,
      image_url: inputProduct.image_url
    };
    axios.patch("/api/products/" + product.id, params).then(response => {
      console.log("Successful update", response.data);
      this.name = response.data.name;
      this.price = response.data.price;
      this.description = response.data.description;
      this.image_url = response.data.image_url;
    });
  },
  destroyProduct: function(inputProduct) {
    axios.delete("/api/products/" + inputProduct.id).then(response => {
      console.log("Deleted successfully", response.data);
      var index = this.products.indexOf(inputProduct);
      this.products.splice(index, 1);
    });
  }
};
</script>
