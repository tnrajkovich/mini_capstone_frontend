<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New property</h2>
    <button v-on:click="sortAttribute = 'name'" type="text">Sort by name</button>
    Search:
    <input v-model="searchFilter" type="text" />
    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>
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
    <button v-on:click="createProperty" class="socrata-icon-arrow-up2 sort-indicator">Create property</button>
    <div v-for="product in orderBy(filterBy(products, searchFilter, 'name'), sortAttribute)">
      <h2>{{ product.name }}</h2>
      <p>{{ product.price }}</p>
      <img v-bind:src="product.image_url" alt="" />
      <div>
        <router-link v-bind:to="`/api/products/${product.id}`">More info</router-link>
        <router-link v-bind:to="`/api/products/${product.id}/edit`">Edit property</router-link>
        <div>
          <transition-group
            class="card"
            v-bind:class="{ selected: currentProduct === product }"
            v-on:click="product = currentProduct"
            appear
            enter-active-class="animated fadeIn"
            leave-active-class="animated fadeOut"
          >
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
          </transition-group>
        </div>
      </div>
      <button v-on:click="deleteProduct(product)">Delete property</button>
    </div>
  </div>
</template>

<style>
.selected {
  color: white;
  background-color: magenta;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      product: {},
      more_info: {},
      inputProduct: {},
      currentProduct: {},
      searchFilter: "",
      sortAttribute: "name"
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
  destroyProduct: function(inputProduct) {
    axios.delete("/api/products/" + inputProduct.id).then(response => {
      console.log("Deleted successfully", response.data);
      var index = this.products.indexOf(inputProduct);
      this.products.splice(index, 1);
    });
  }
};
</script>
