<script>
import { store } from "../store/store.js"
export default {

data() {
  return {
    store: store,
    subTotal: 0,
    accumulater: 0,
    windowWidth: window.innerWidth
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
}, computed: {
    getSubtotal(){
        this.accumulater = 0
    for (let key in this.store.cartItemsObj) {
        if (this.store.cartItemsObj.hasOwnProperty(key)) {
            this.accumulater += this.store.cartItemsObj[key].quantity * this.store.cartItemsObj[key].regular_price.value
        }
    }
    return parseFloat(this.accumulater).toFixed(2)
    }
}
}

</script>

<template>
    <main>
        <h1>
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
    <li class="product-card" v-for="(data, index) in store.cartItemsObj" v-if="windowWidth > 700">
        
        <div class="item-column" >
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
            {{ data.regular_price.currency }}
            {{ data.regular_price.value }}
        </span>
        <span class="product-quantity">
            <button @click="removeProduce(data.title)" v-if="data.quantity>0">-</button>
            {{ data.quantity }}
            <button @click="addProduct(data.title)">+</button>
        </span>
        <span class="product-price">
            {{ data.regular_price.currency }}
            {{ parseFloat(data.regular_price.value * data.quantity).toFixed(2) }}
        </span>
        <img class="trash-image" src="../assets/vue-test-master/assets/images/trash.png" @click="deleteCartProduct(data.title, data.quantity)"> 

    </li>

    <li class="product-card" v-for="(data, index) in store.cartItemsObj" v-else>
            <div class="thumbnail-wrapper">
                <img v-bind:src="`${data.image}`" class="product-image">
            </div>
            <div class="product-data-wrapper">
                <span class="product-title">
                Title: {{ data.title }}
            </span>
            <span class="product-brand">
                Brand: {{ data.brand }}
            </span>
        <span class="product-currency">
            Price:
            {{ data.regular_price.currency }}
            {{ data.regular_price.value }}
        </span>
        <span class="product-quantity">
            Quantity:
            <button @click="removeProduce(data.title)" v-if="data.quantity>0">-</button>
            
            {{ data.quantity }}
            <button @click="addProduct(data.title)">+</button>
        </span>
        <span class="product-price">
            Total price:
            {{ data.regular_price.currency }}
            {{ parseFloat(data.regular_price.value * data.quantity).toFixed(2) }}
        </span>
        <button class="remove-from-cart" @click="deleteCartProduct(data.title, data.quantity)">
            <span>
                Remove
            </span>
            <img class="trash-image" src="../assets/vue-test-master/assets/images/trash.png" > 
        </button>
    </div>
            

    </li>
   
  </ul>
  <div id="checkout-wrapper">
    <h2 id="accumulated-price">Subtotal: USD {{ getSubtotal }}</h2>
    <button id="checkout">Checkout</button>
  </div>
  
    </main>
</template>

<style scoped>
main {
    margin-left: auto;
    margin-right: auto;
    max-width: 1400px;
    position: relative;
}

.product-data-wrapper {
    display: flex;
    flex-direction: column;
    width: 100%;
}

.remove-from-cart {
    display: flex;
    align-items: center;
    padding: 6px;
    width: 70%;
}

.remove-from-cart > span{
    width: 100%;
}


#product-data-columns {
    display: flex;
    width: 100%;
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


.product-image {
    max-height: 600px;
    max-width: 200px;
}

@media (max-width: 700px) {
    .product-image {
        max-height: 100px;
        max-width: 100px;
    }
    .product-card {
        border-bottom: 4px solid black;
        padding: 10px;
    }
    .product-details {
        display: flex;
        flex-direction: row;
        width: 100%;
    }
    .product-data-wrapper {
        margin-left: 30px;
        gap: 5px;
    }
    .product-data-wrapper > span {
        text-align: left;
        width: 100%;
        
    }
    body .trash-image {
        height: 1em;
        width: 1em;
        cursor: pointer;
        text-align: center;
        margin: auto;
    }
}

.trash-image {
    height: 50px;
    width: 50px;
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

#checkout-wrapper {
    position: absolute;
    right: 5%;
}

#checkout {
    background: none;
    width: 100%;
}
</style>