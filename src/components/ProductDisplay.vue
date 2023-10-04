<template>
  <div class="container">
    <div v-if="isLoading" class="card">
      <div class="loader"></div>
    </div>
    <div
      v-else
      :class="productContainerClass"
      class="container"
    >
      <div class="overlay">
        <img src="../assets/bg-pattern.png" />
      </div>
      <div class="card">
        <div v-if="!isProductAvailable" class="product-unavailable">
          <div class="overlay">
            <img
              src="../assets/sad-emoji.svg"
            />
          </div>
          <div class="product-details">
            <p>This product is unavailable to show</p>
            <div class="button-container">
              <button
                type="button"
                @click="fetchNextProduct()"
                class="btn-next"
              >
                Next Product
              </button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
    <div class="product-image">
      <img :src="product.data.image" />
    </div>
    <div class="product-details">
      <div class="product-title">
        <h3
          :class="
            product.data.category === 'men\'s clothing'
              ? 'font-navy'
              : 'font-pink-dark'
          "
          class="title"
        >
          {{ product.data.title }}
        </h3>
        <div class="product-sub-title">
          <span>{{ product.data.category }}</span>
          <div>
            <span>{{ product.data.rating.rate }}/5</span>
          </div>
        </div>
        <div class="description">
          <p>{{ product.data.description }}</p>
        </div>
      </div>
      <div class="price-color">
        <span
          :class="
            product.data.category === 'men\'s clothing'
              ? 'font-navy'
              : 'font-pink-dark'
          "
          class="price"
          >${{ product.data.price }}</span
        >
        <div class="button-container">
          <button
            type="button"
            :class="
              product.data.category === 'men\'s clothing'
                ? 'bg-navy'
                : 'bg-pink-dark'
            "
            class="btn-buy"
          >
            Buy Now
          </button>
          <button
            type="button"
            @click="fetchNextProduct()"
            :class="productCategoryBorderClass"
            class="btn-next"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>
  </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductDisplay',
  data() {
    return {
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      product: {}
    };
  },
  computed: {
    productContainerClass() {
      return {
        'bg-light-gray': !this.isProductAvailable,
        'bg-light-blue': this.isProductAvailable && this.product.data.category === "men's clothing",
        'bg-light-pink': this.isProductAvailable && this.product.data.category !== "men's clothing"
      };
    },
    productTitleClass() {
      return this.product.data.category === "men's clothing" ? 'font-navy' : 'font-pink-dark';
    },
    productCategoryClass() {
      return this.product.data.category === "men's clothing" ? 'bg-navy' : 'bg-pink-dark';
    },
    productCategoryBorderClass() {
      return this.product.data.category === "men's clothing" ? 'border-btn-navy font-navy' : 'border-btn-pink-dark font-pink-dark';
    }
  },
  methods: {
    async fetchProductData() {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
      const data = await response.json();

      return data;
    },
    async fetchNextProduct() {
      this.isLoading = true;

      this.index = this.index === 20 ? 1 : this.index + 1;

      const data = await this.fetchProductData();
      
      this.isProductAvailable = data.category === "men's clothing" || data.category === "women's clothing";

      if (this.isProductAvailable) {
        this.product = { data };
      }

      this.isLoading = false;
    }
  },
  mounted() {
    this.fetchNextProduct();
  }
};
</script>

<style scoped>
@import '../assets/style/page.css';
</style>
