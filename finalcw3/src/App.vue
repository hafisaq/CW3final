<template>
  <div id="app">
    <HeaderCheck :cartItemsCount="cartItemCount" @toggle-checkout="toggleCheckout" />
    <div class="container">
      <LessonList v-if="!showCheckout" @add-to-cart="addToCart" :products="products" />
      <LessonCheckout v-if="showCheckout" :cart="cart" @remove-from-cart="removeFromCart" @back-to-lessons="backToLessons" />
    </div>
  </div>
</template>

<script>
import LessonList from './components/NewLesson.vue';
import LessonCheckout from './components/LessonCheckout.vue';
import HeaderCheck from './components/Header.vue';
import { products } from './products.js';

export default {
  name: 'App',
  components: {
    LessonList,
    LessonCheckout,
    HeaderCheck,
  },
  data() {
    return {
      products: products,
      cart: [],
      showCheckout: false,
    };
  },
  computed: {
    cartItemCount() {
      let count = 0;
      for (const item of this.cart) {
        count += item.quantity;
      }
      return count;
    },
  },
  methods: {
    addToCart(productId) {
      const product = this.products.find((p) => p.id === productId);
      if (product.availableInventory > 0) {
        const cartItem = this.cart.find((item) => item.id === productId);
        if (cartItem) {
          cartItem.quantity++;
        } else {
          this.cart.push({ ...product, quantity: 1 });
        }
        product.availableInventory--;
      }
    },
    removeFromCart(productId) {
      const index = this.cart.findIndex((item) => item.id === productId);
      if (index !== -1) {
        const cartItem = this.cart[index];
        cartItem.quantity--;
        if (cartItem.quantity <= 0) {
          this.cart.splice(index, 1);
        }
        const product = this.products.find((p) => p.id === productId);
        product.availableInventory++;
      }
    },
    toggleCheckout() {
      this.showCheckout = !this.showCheckout;
    },
    backToLessons() {
      this.showCheckout = false;
    },
  },
};
</script>
<style>
 body {
    font-family: Arial, sans-serif;
    background-color: #15202b;
    color: #ffffff;
  }

  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 15px;
  }

  button {
    cursor: pointer;
  }

  input, select {
    display: block;
    width: 100%;
    padding: 6px 12px;
    margin-bottom: 10px;
    font-size: 14px;
    line-height: 1.42857143;
    color: #ffffff;
    background-color: #243447;
    background-image: none;
    border: 1px solid #2e4259;
    border-radius: 4px;
  }
</style>
