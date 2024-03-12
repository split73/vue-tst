<script>
import { store } from "../store/store.js"
export default {

setup() {
  return {
    store: store
  }
},
created() {
},
methods: {
    addProduct(title) {
        this.store.cartItemsObj[title].quantity++
        this.store.cartAmount++
    },
    removeProduce(title) {
        this.store.cartItemsObj[title].quantity--
        this.store.cartAmount--
    },
    deleteCartProduct(title) {
        this.store.cartAmount -= this.store.cartItemsObj[title].quantity;
        delete this.store.cartItemsObj[title]
    }
}
}

</script>

<template>
    <main>
        <h1>
            Shopping Cart
        </h1>
            <div id="product-data-columns" >
                <span class="item-product-data-column">
                    Item
                </span>
                <span class="price-product-data-column">
                    Price
                </span>
                <span  class="qty-product-data-column">
                    Qty
                </span>
                <span  class="total-product-data-column">
                    Total
                </span>
            </div>
            
  <ul id="product-container"  >
    <li class="product-card" v-for="(data, index) in store.cartItemsObj">
        
        <div class="item-column">
            <div class="thumbnail-wrapper">
                <img v-bind:src="`${data.image}`" class="product-image">
            </div>

            <span class="product-title">
                {{ data.title }}
            </span>
            <span class="product-brand">
                {{ data.brand }}
            </span>
        </div>
        <span class="product-currency">
          {{ data.regular_price.value }}
        </span>
        <span class="product-quantity">
            <button @click="removeProduce(data.title)">-</button>
            {{ data.quantity }}
            <button @click="addProduct(data.title)">+</button>
        </span>
        <span class="product-price">
            {{ data.regular_price.currency }}
            {{ data.regular_price.value * data.quantity }}
        </span>
        <img class="trash-image" src="../assets/vue-test-master/assets/images/trash.png" @click="deleteCartProduct(data.title)"> 

    </li>
  </ul>
    </main>
</template>

<style scoped>
main {
    margin-left: auto;
    margin-right: auto;
    max-width: 1400px;
}

#product-data-columns {
    display: flex;
    width: 100%;
    /* justify-content: end; */
    text-align: center;
}

.item-product-data-column {
    width: 32%;
    margin-right: 18%;
}

.product-currency, .product-quantity, .product-price {
    width: 15%;
    text-align: center;
}

#product-container {
    display: flex;
    flex-direction: column;
    width: calc(100% - 0px);
    flex-wrap: wrap;
    gap: 10px;
    padding: 0;
}

  .product-card {
      display: flex;
      flex-direction: row;
      width: 100%;
      align-items: center;
      min-width: 200px;
      border-bottom: 4px solid black;
  }

@media (max-width: 768) {
  .product-card {
    border-bottom: 4px solid black;
    
  }
}

@media (max-width: 415px) {
    .product-image {
    max-height: 100px;
    max-width: 100px;
    }
}

.product-image {
    max-height: 600px;
    max-width: 200px;
}

@media (max-width: 570px) {
    .product-image {
    max-height: 75px;
    max-width: 75px;
    }
}

.trash-image {
    height: 50px;
    width: 5%;
    cursor: pointer;
}

.product-details {
  display: flex;
  flex-direction: row;
  width: 10%;
}

li {
  list-style: none;
}

.item-column {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 50%;
}

.price-product-data-column, .qty-product-data-column, .total-product-data-column {
    width: 15%;
}
</style>