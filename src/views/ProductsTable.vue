<template>
  <div class="products-index">
    <h1>All Products</h1>
    <div>
      Product Name: <input v-model="nameFilter" list="names">

      <datalist id="names">
        <option v-for="product in products">{{product.name}}</option>
      </datalist>
    </div>


    <table class="table table=striped">
      <thead>
        <tr>
          <th scope="col" v-on:click="setSortAttribute('id')">Sort by ID{{ orderIndicator('id') }}</th>
          <th scope="col" v-on:click="setSortAttribute('name')">Sort by Name{{ orderIndicator('name') }}</th>
          <th scope="col" v-on:click="setSortAttribute('formatted.price')">Sort by Price{{ orderIndicator('formatted.price') }}</th>
        </tr>   
      </thead>
      <tr v-for="product in orderBy(filterBy(products, nameFilter, 'name'), sortAttribute, sortOrder)">
        <!-- filtering and ordering at the same time -->
        <td>
          {{ product.id }}
        </td>
        <td>
          <router-link v-bind:to="'/products/' + product.id">
            {{ product.name }}
          </router-link>
        </td>
        <td>
          {{ product.formatted.price }}
        </td>
      </tr>
    </table>


  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');
import Vue2Filters from "vue2-filters"

export default {
  data: function() {
    return {
      products: [],
      currentProduct: {},
      nameFilter: '',
      sortAttribute: 'name',
      sortOrder: 1
    }
  },
  created: function () {
    axios.get("/api/products")
    .then(response => {
      this.products = response.data
    });
  },
  methods: {
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        this.sortOrder *= -1;
      } else {
        this.sortAttribute = inputAttribute; 
        this.sortOrder = 1;
      }
    }
  },
  orderIndicator: function(inputAttribute) {
    if (this.sortAttribute === inputAttribute) {
      if (this.sortOrder === 1) {
        return "▼";
      } else {
        return "▲";
      }
    } else {
      return " ";
    }
  },
  mixins: [Vue2Filters.mixin]
};
</script>