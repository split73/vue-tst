<script>
import productsJSON from "../assets/vue-test-master/products.json";
import logo from "../assets/vue-test-master/assets/images/logo.png"
import { store } from "../store/store.js"

export default {
  data() {
    return {
      products: productsJSON,
      logo: logo,
      clicked: false,
      filterSet: new Set(),
      store: store,
    }
  },
  created() {
    
  },
  methods: {
    addBrandToFilter(product){
      if (this.filterSet.has(product.brand)){
        this.filterSet.delete(product.brand)
      } else {
        this.filterSet.add(product.brand)
      }
      return this.filterSet.has(product.brand)
    },
    addToCart(product) {
      this.store.cartAmount++
      
      if (Object.hasOwn(this.store.cartItemsObj, product.title)){
        this.store.cartItemsObj[product.title].quantity++
      } else {
        product.quantity = 1
        this.store.cartItemsObj[product.title] = product
      }
    }
    
  },
  computed: {
    filteredByBrand() {
      if (this.filterSet.size === 0){
        return this.products
      }

      return this.products.filter((product) => 
        this.filterSet.has(product.brand)
      )
    }
  }
}

</script>

<template>
 
  <main>
    <aside id="sidebar-filters">
      <span id="filters-title">
        filters
      </span>
      <ul id="filters-wrapper" v-for="product in products">
        <li :class="{ filter: true, activeFilter: this.filterSet.has(product?.brand) }">
          <button 
            v-on:click ="addBrandToFilter(product)"
            class="filter-selector">
            brand {{ product.brand }}
          </button>
        </li>
      </ul>
    </aside>

  <ul id="product-container"  >
    <li class="product-card" v-for="product in filteredByBrand" @click="addToCart(product)">
      <div class="thumbnail-wrapper">
        <img v-bind:src="`${product.image}`" class="product-image" draggable="false">
      </div>
      <div class="product-details">
        <span class="product-title">
        {{ product.title }}
        </span>
        <span class="product-brand">
          {{ product.brand }}
        </span>
        <span class="product-price">
          {{ product.regular_price.currency }}
          {{ product.regular_price.value }}
        </span>
      </div>
    </li>
  </ul>

  </main>
</template>

<style scoped>
li {
  list-style: none;
}

.activeFilter {
  border-bottom: 2px solid black;
}

main {
  display: flex;
  flex-direction: row;
  margin: 30px;
  flex-wrap: wrap;
  max-width: 1400px;
  margin: auto;
}

#sidebar-filters{
  display: flex;
  flex-direction: column;
  width: 50px;
}

.filter-selector {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
}

#filters-wrapper{
  margin: 0;
  padding: 0;
}

#product-container {
    display: flex;
    flex-direction: row;
    width: calc(100% - 90px);
    flex-wrap: wrap;
    gap: 10px;
    cursor: pointer;
}
@media (min-width: 805px) {
  .product-card {
      display: flex;
      flex-direction: column;
      width: calc(33.3% - 10px);
      align-items: center;
      min-width: 200px;
  }
}
@media (max-width: 805px) {
  .product-card {
    width: calc(50% - 10px);
  }
}
.product-image {
    max-height: 600px;
    width: 100%;
    
}
.product-details {
  display: flex;
  flex-direction: column;
}
</style>
