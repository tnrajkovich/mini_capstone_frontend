<template>
  <div class="container show">
    <p>Name: {{ inputProduct.name }}</p>
    <p>Price: {{ inputProduct.price }}</p>
    <p>Description: {{ inputProduct.description }}</p>
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
    axios.get("/api/products/" + this.$router.inputProduct.id + "/edit").then(response => {
      this.product = response.data;
      console.log(this.product);
    });
  },
  methods: {
    updateProduct: function(inputProduct) {
      var params = {
        name: inputProduct.name,
        price: inputProduct.price,
        description: inputProduct.description,
        image_url: inputProduct.image_url
      };
      axios.patch("/api/products/" + inputProduct.id, params).then(response => {
        console.log("Successful update", response.data);
        this.name = response.data.name;
        this.price = response.data.price;
        this.description = response.data.description;
        this.image_url = response.data.image_url;
      });
    }
  }
};
</script>
