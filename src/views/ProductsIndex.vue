<template>
  <div class="products-index">
    <h1>All Products</h1>
    <div>
      Product Name: <input v-model="titleName" list="names">

      <datalist id="names">
        <option v-for="product in products">{{product.name}}</option>
      </datalist>
    </div>

    <div v-for="product in filterBy(products, titleName, 'name')">
      <h2>{{ product.name}}</h2>
      <router-link v-bind:to="'/products/' + product.id">
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      </router-link>
    </div>
  </div>
</template>

<style>
/*  div {
    width: auto;
    padding: 10px;
    border: 5px solid black;
    margin: 0; 
  }*/

  img {
    width: 400px;
  }

</style>

<script>
var axios = require('axios');
import Vue2Filters from "vue2-filters"

export default {
  data: function() {
    return {
      products: [],
      currentProduct: {},
      titleName: ''
    }
  },
  created: function () {
    axios.get("/api/products")
    .then(response => {
      this.products = response.data
    });
  },
  methods: {

  },
  mixins: [Vue2Filters.mixin]
};
</script>
