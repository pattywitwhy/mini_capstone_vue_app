<template>
  <div class="product-show">
    <h1>{{ product.title }}</h1>
    <h4>Name: {{ product.name }}</h4>
    <h4>Description: {{ product.description }}</h4>
    <p>Price: {{ product.price }}</p>
    <img v-bind:src="product.image_url" v-bind:alt="product.title">

    <router-link :to=" '/products/' + product.id + '/edit' ">Edit</router-link>
    <button v-on:click="destroyProduct()">Delete</button>
  </div>
</template>

<style></style>

<script>
var axios = require('axios');

export default {
  data: function () {
    return {
      product: {
                id: "",
                name: "",
                description: "",
                image_url: "",
                price: "",
                tax: "",
                total: "",
                formatted: {
                            price: "",
                            tax: "",
                            total: "" }
     }
    };
  },
  created: function () {
    axios.get("/api/products/" + this.$route.params.id)
    .then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  }, 
  methods: {
    destroyProduct: function(inputProduct) {
      axios.delete("api/products/" + this.product.id)
        .then(response => {
          console.log("Success", response.data);
          this.$router.push("/");
        });
    }
  }
}
</script>