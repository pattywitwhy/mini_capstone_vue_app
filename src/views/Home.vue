<template>
  <div class="home">
    <h1>New Product</h1>
    <div>
      <div>
        Name: <input v-model="newProductName">
      </div>
      <div>
        Description: <input v-model="newProductDescription">
      </div>
      <div>
        Price: <input v-model="newProductPrice">
      </div>
      <div>
        Image URL: <input v-model="newProductImageUrl">
      </div>
      <button v-on:click="createProduct()">Create</button>
    </div>

    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name}}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.title">
      <div>
        <button v-on:click="currentProduct = product">More Info</button>
      </div>
      <div v-if="product === currentProduct">
        <h4>{{ product.description}}</h4>
        <h4>{{ product.formatted.price}}</h4>

        <div>
          <h4>Edit Product</h4>
          <div>
            <div>
              Name: <input v-model="product.name">
            </div>
            <div>
              Description: <input v-model="product.description">
            </div>
            <div>
              Price: <input v-model="product.formatted.price">
            </div>
            <div>
              Image URL: <input v-model="product.image_url">
            </div>
            <button v-on:click="updateProduct(product)">Update</button>
            <button v-on:click="destroyProduct(product)">Delete</button>
          </div>
        </div>
      </div>
      
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

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {}
    }
  },
  created: function () {
    axios.get("/api/products")
    .then(response => {
      this.products = response.data
    });
  },
  methods: {
    createProduct: function() {
      console.log("Create the Product...")
      var params = {
                    name: this.newProductName,
                    description: this.newProductDescription,
                    price: this.newProductPrice,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params)
        .then(response => {
          console.log("Success", response.data)
          this.products.push(response.data);
        });
    },
    updateProduct: function(inputProduct) {
      var params = {
                    name: inputProduct.name,
                    description: inputProduct.description,
                    price: inputProduct.formatted.price,
                    image_url: inputProduct.image_url
                    };
      axios.patch("/api/products/" + inputProduct.id, params)
        .then(response => {
          console.log("Success", response.data)
          inputProduct = response.data;
        });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("api/products/" + inputProduct.id)
        .then(response => {
          console.log("Success", response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index, 1);
        });
    }
  }
};
</script>
