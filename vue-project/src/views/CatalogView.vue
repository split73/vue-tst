<script>
import productsJSON from "../assets/vue-test-master/products.json";
import logo from "../assets/vue-test-master/assets/images/logo.png"
import { isProxy, toRaw } from 'vue';

export default {

  data() {
    return {
      products: productsJSON,
      logo: logo,
      clicked: false,
      qq: {q: 10}
    }
  },
  created() {
    console.log(this.products)
  },
  methods: {
    log(brand){
      console.log(brand)
    },
    underLine(brand){
      console.log(toRaw(this.products))
      this.products = this.products[0]
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
      <ul id="filters-wrapper" v-for="data in products">
        <li :id="data.brand" class="filter">
          <button 
            v-on:click ="underLine(data.brand)"
            class="filter-selector">
            brand {{ data.brand }}
          </button>
        </li>
      </ul>
    </aside>

  <ul id="product-container"  >
    <li class="product-card" v-for="data in products">
      <div class="thumbnail-wrapper">
        <img v-bind:src="`${data.image}`" class="product-image">
      </div>
      <div class="product-details">
        <span class="product-title">
        {{ data.title }}
        </span>
        <span class="product-brand">
          {{ data.brand }}
        </span>
        <span class="product-price">
          {{ data.regular_price.currency }}
          {{ data.regular_price.value }}
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
