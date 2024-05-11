<template>
  <div class="main">
    <div v-if="product">
      <div v-bind:class="getClassByCategory(product.category)">
        <h1>{{ product.category }}</h1>
        <h1>{{ product.title }}</h1>
        <img v-bind:src="product.image" alt="Product Image">
        <p>{{ product.description }}</p>
        <p>Price: ${{ product.price }}</p>
        <p>Rating: {{ product.rating.rate }}</p>
      </div>
      <button @click="getNextProduct">Next Product</button>
    </div>
    <div v-else>
      <p>No product available.</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: null,
      index: 1
    };
  },
  methods: {
    async fetchProduct(index) {
      try {
        const response = await fetch(`https://fakestoreapi.com/products/${index}`);
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error fetching product:', error);
      }
    },
    getClassByCategory(category) {
      if (category === "men's clothing") {
        return 'men-section';
      } else if (category === "women's clothing") {
        return 'women-section';
      } else {
        return 'unavailable-product';
      }
    },
    async getNextProduct() {
      this.index = (this.index % 20) + 1; // Loop back to 1 when index reaches 20
      this.product = await this.fetchProduct(this.index);
    }
  },
  async mounted() {
    this.product = await this.fetchProduct(this.index);
  }
};
</script>

<style scoped>
/* Vanilla CSS styles */
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --error-color: #dc3545;
  --background-color: #f8f9fa;
}

.men-section {
  /* Add men section styles here */
}

.women-section {
  /* Add women section styles here */
}

.unavailable-product {
  display: none; /* Menggunakan display: none; untuk menyembunyikan elemen */
}
</style>

