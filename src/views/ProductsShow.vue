<template>
  <div class="container show">
    <p>Name: {{ product.name }}</p>
    <p>Price: {{ product.price }}</p>
    <p>Description: {{ product.description }}</p>
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
      product: {},
      inputProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$router.params.id).then(response => {
      this.product = response.data;
      console.log(this.product);
    });
  },
  methods: {}
};
</script>
