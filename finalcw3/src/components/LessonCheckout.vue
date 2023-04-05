<template>
    <div>
      <h2>Lesson Checkout</h2>
      <div v-if="cart.length > 0">
        <p>Your cart:</p>
        <ul>
          <li v-for="i in cart" :key="i.id" id="cart">
    <figure>
      <img :src="i.image" style="height: 50px" />
    </figure>
    <p>{{ i.title }}</p>
    <p>{{ i.description }}</p>
    <p>{{ i.price }}</p>
    <button @click="removeFromCart(i.id)">Remove</button>
  </li>
        </ul>
        <p>Total: ${{ total }}</p>
        <form @submit.prevent="submitForm">
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="name" :class="{ invalid: !nameIsValid }" required>
          <label for="confirmName">Confirm Name:</label>
          <input type="text" id="confirmName" v-model="confirmName" :class="{ invalid: !confirmNameIsValid }" required>
          <label for="address">Address:</label>
          <input type="text" id="address" v-model="address" :class="{ invalid: !addressIsValid }" required>
          <label for="phone">Phone Number:</label>
          <input type="text" id="phone" v-model="phone" :class="{ invalid: !phoneIsValid }" required>
          <button :disabled="!formIsValid">Checkout</button>
        </form>
        <button @click="backToLessons">Back to Lessons</button>
      </div>
      <div v-else>
        <p>Your cart is empty</p>
        <button @click="backToLessons">Back to Lessons</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      cart: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        name: '',
        confirmName: '',
        address: '',
        phone: '',
        nameIsValid: false,
        confirmNameIsValid: false,
        addressIsValid: false,
        phoneIsValid: false,
      };
    },
    computed: {
      total() {
        return this.cart.reduce((acc, item) => acc + item.price * item.quantity, 0);
      },
      formIsValid() {
        return this.nameIsValid && this.confirmNameIsValid && this.addressIsValid && this.phoneIsValid;
      },
    },
    methods: {
      removeFromCart(productId) {
        this.$emit('remove-from-cart', productId);
      },
      backToLessons() {
        this.$emit('back-to-lessons');
      },
      submitForm() {
        if (!this.formIsValid) {
          alert('Please fill out all fields correctly');
          return;
        }
  
        // Clear the cart
        this.$emit('clear-cart');
  
        // Show thank you message
        alert('Thank you for your purchase!');
  
        // Reset the form fields
        this.name = '';
        this.confirmName = '';
        this.address = '';
        this.phone = '';
        this.nameIsValid = false;
        this.confirmNameIsValid = false;
        this.addressIsValid = false;
        this.phoneIsValid = false;
        location.reload();
        
      },
    },
    watch: {
      name() {
        this.nameIsValid = /^[a-zA-Z]+$/.test(this.name);
      },
      confirmName() {
        this.confirmNameIsValid = /^[a-zA-Z]+$/.test(this.confirmName) && this.name === this.confirmName;
      },
      address() {
        this.addressIsValid = this.address.length >= 2;
      },
  
  
      phone() {
          this.phoneIsValid = /^\d{10}$/.test(this.phone);
        },
      },
    };
  </script>
  
  <style>

  /* Checkout styles */
  .invalid {
    border-color: red;
  }

  .checkout button {
    background-color: #1da1f2;
    border: none;
    border-radius: 4px;
    color: #ffffff;
    font-size: 14px;
    font-weight: 600;
    padding: 12px 20px;
    margin-bottom: 20px;
    transition: background-color 0.3s, transform 0.3s;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  .checkout button:hover {
    background-color: #0c86d0;
    transform: translateY(-2px);
  }

  .checkout button:active {
    background-color: #0a6cad;
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .checkout ul {
    list-style-type: none;
    padding: 0;
    margin-bottom: 20px;
  }

  .checkout li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #2e4259;
  }

  .checkout li:last-child {
    border-bottom: none;
  }


 </style>
  
       
  