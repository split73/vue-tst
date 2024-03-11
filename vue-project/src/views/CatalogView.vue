<script>
import productsJSON from "../assets/vue-test-master/products.json";
import logo from "../assets/vue-test-master/assets/images/logo.png"

export default {
  data() {
    return {
      products: productsJSON,
      logo: logo,
      clicked: false,
      filterSet: new Set(),
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
 
  <main id="main">
    <aside id="sidebar-filters">
      <span id="filters-title">
        filters
      </span>
      <ul id="filters-wrapper" v-for="product in products">
        <li :id="product.brand" class="filter">
          <button 
            v-on:click ="addBrandToFilter(product)"
            class="filter-selector">
            brand {{ product.brand }}
          </button>
        </li>
      </ul>
    </aside>

  <ul id="product-container"  >
    <li class="product-card" v-for="product in filteredByBrand">
      <div class="thumbnail-wrapper">
        <img v-bind:src="`${product.image}`" class="product-image">
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
.blue {
  background-color: aqua;
}
li {
  list-style: none;
}


#main {
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


.filter {
  border-bottom: 2px solid black;
}
</style>
