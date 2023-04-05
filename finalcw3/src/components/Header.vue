<template>
    <header>
      <div class="container">
        <h1>Tuition Center</h1>
        <button v-if="cartItemsCount" @click="toggleCheckout">
          {{ cartItemsCount }}
          <span class="fa fa-shopping-cart"></span>
        </button>
      </div>
    </header>
  </template>
  
  <script>
export default {
  name: "HeaderCheck",
  props: {
    cartItemsCount: {
      type: Number,
      required: true,
    },
    isCheckoutVisible: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    toggleCheckout() {
      this.$emit("toggle-checkout");
    },
  },
};
  </script>
  
  <style>
   header {
    background-color: #192734;
    padding: 15px 0;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }

  header h1 {
    display: inline;
    font-size: 24px;
    margin: 0;
  }

  header button {
    display: inline;
    float: right;
    background-color: #1da1f2;
    border: none;
    border-radius: 4px;
    color: #ffffff;
    font-size: 16px;
    padding: 10px 15px;
    transition: background-color 0.3s;
  }

  header button:hover {
    background-color: #0c86d0;
  }

  header .fa-shopping-cart {
    margin-left: 5px;
  }
  </style>
  