<template>
    <div class="lesson-list">
      <div class="search-bar">
        <input type="text" v-model="searchQuery" placeholder="Search lessons by name or location" />
        <select v-model="sortField" @change="updateSortedProducts">
          <option value="title">Title</option>
          <option value="price">Price</option>
          <option value="availableInventory">Available Inventory</option>
        </select>
        <select v-model="sortOrder" @change="updateSortedProducts">
          <option value="asc">Ascending</option>
          <option value="desc">Descending</option>
        </select>
      </div>
      <div v-for="product in sortedProducts" :key="product.id" class="lesson">
        <h3>{{ product.title }}</h3>
        <figure>
          <img v-bind:src="product.image" style="height: 80px" alt="Productimage" />
  
        </figure>
        <p>{{ product.description }}</p>
        <p>Price: {{ product.price }}</p>
        <p>Location: {{ product.location }}</p>
        <p>Rating: {{ product.rating }}</p>
        <p v-if="product.availableInventory < 5">
          Only {{ product.availableInventory }} left
        </p>
        <p v-else>Available Inventory: {{ product.availableInventory }}</p>
        <button
          @click="$emit('add-to-cart', product.id)"
          :disabled="product.availableInventory <= 0"
        >
          Add to cart
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "LessonList",
    props: ["products"],
    data() {
      return {
        searchQuery: "",
        sortField: "title",
        sortOrder: "asc",
      };
    },
    computed: {
      sortedProducts() {
        let sortedProducts = [...this.products]; // create a copy of the products array
        if (this.sortField === "title") {
          sortedProducts.sort((a, b) => a.title.localeCompare(b.title));
        } else if (this.sortField === "price") {
          sortedProducts.sort((a, b) => a.price - b.price);
        } else if (this.sortField === "availableInventory") {
          sortedProducts.sort(
            (a, b) => a.availableInventory - b.availableInventory
          );
        }
  
        if (this.sortOrder === "desc") {
          sortedProducts.reverse();
        }
  
        if (this.searchQuery) {
          sortedProducts = sortedProducts.filter(
            (product) =>
              product.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
              product.location.toLowerCase().includes(this.searchQuery.toLowerCase())
          );
        }
  
        return sortedProducts;
      },
    },
    methods: {
      updateSortedProducts() {
        this.sortedProducts; // this will trigger the computed property to recalculate
      },
    },
  };
  </script>

    <style>.lesson-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
      }
    
      .lesson-list .search-bar {
        display: flex;
        justify-content: space-between;
        width: 100%;
        margin-bottom: 20px;
      }
    
      .lesson {
        width: calc(33% - 20px);
        margin-bottom: 30px;
      }
    
      .lesson h3 {
        font-size: 20px;
        margin: 0 0 10px;
      }
    
      .lesson p {
        margin: 0 0 10px;
      }
    
      .lesson button {
        background-color: #1da1f2;
        border: none;
        border-radius: 4px;
        color: #ffffff;
        font-size: 14px;
        padding: 10px 15px;
        transition: background-color 0.3s;
      }
    
      .lesson button:hover {
        background-color: #0c86d0;
      }</style>  
    
