<template>
  <div v-bind:class="getClassByCategory(product.category)">
    <div v-if="product && (product.category === 'men\'s clothing' || product.category === 'women\'s clothing')"
      class="content">
      <div class="left">
        <img v-bind:src="product.image" alt="Product Image">
      </div>
      <div class="right">
        <div class="header">
          <h1>{{ product.title }}</h1>
          <div class="rate">
            <h2>{{ product.category }}</h2>
            <div class="rating">
              <p>{{ product.rating.rate }}/5</p>
              <div class="circle"></div>
              <div class="circle"></div>
              <div class="circle"></div>
              <div class="circle1"></div>
              <div class="circle1"></div>
            </div>
          </div>
          <div class="line"></div>
          <p class="desc">{{ product.description }}</p>
        </div>
        <div class="bottom">
          <div class="line"></div>
          <p>${{ product.price }}</p>
          <div class="btn">
            <button class="buy-btn">Buy now</button>
            <button @click="getNextProduct" class="next-btn" :disabled="loading">
              <span v-if="loading">Loading...</span>
              <span v-else>Next Product</span>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="content">
      <p>This product is unavailable to show</p>
      <button @click="getNextProduct" class="next-btn" :disabled="loading">
        <span v-if="loading">Loading...</span>
        <span v-else>Next Product</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: null,
      index: 1,
      loading: false
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
      this.loading = true;

      try {
        this.index = (this.index % 20) + 1; 
        this.product = await this.fetchProduct(this.index);
      } catch (error) {
        console.error('Error fetching product:', error);
      } finally {
        this.loading = false;
      }
    }
  },
  async mounted() {
    this.product = await this.fetchProduct(this.index);
  }
};
</script>

<style src="../assets/style.css"></style>
