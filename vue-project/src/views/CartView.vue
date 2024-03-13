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
        addProduct(id) {
            this.store.cartItemsObj[id].quantity++
            this.store.cartAmount++

        },
        removeProduce(id) {
            this.store.cartItemsObj[id].quantity--
            this.store.cartAmount--
        },
        deleteCartProduct(product) {
            if (product.selectedOption) {
                this.store.cartAmount -= this.store.cartItemsObj[product.selectedOption.product.id].quantity;
                delete this.store.cartItemsObj[product.selectedOption.product.id]
            } else {
                this.store.cartAmount -= this.store.cartItemsObj[product.id].quantity;
                delete this.store.cartItemsObj[product.id]
            }
        }
    }, computed: {
        getSubtotal() {
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
        <div id="product-data-columns" v-if="windowWidth > 700">
            <span class="item-product-data-column">
                Item
            </span>
            <span class="price-product-data-column">
                Price
            </span>
            <span class="qty-product-data-column">
                Qty
            </span>
            <span class="total-product-data-column">
                Total
            </span>
        </div>

        <ul id="product-container">
            <li class="product-card" v-for="product in store.cartItemsObj" v-if="windowWidth > 700">
                <div class="item-column">
                    <div class="thumbnail-wrapper">
                        <img v-if="product.selectedOption" v-bind:src="`${product.selectedOption.product.image}`"
                            class="product-image">
                        <img v-else v-bind:src="`${product.image}`" class="product-image">
                    </div>
                    <span class="product-title">
                        {{ product.title }}
                    </span>
                    <span class="product-brand">
                        {{ product.brand }}
                    </span>
                    <div v-if="product.selectedOption" class="product-options">
                        <span v-for="value in product.configurable_options[0].values">
                            <span v-if="value.value_index === product.selectedOption.attributes[0].value_index"
                                class="product-color">
                                Color:
                                {{ value.label }}
                            </span>
                        </span>
                        <span v-for="value in product.configurable_options[1].values">
                            <span v-if="value.value_index === product.selectedOption.attributes[1].value_index"
                                class="product-size">
                                Size:
                                {{ value.label }}
                            </span>
                        </span>
                    </div>
                </div>
                <span class="product-currency">
                    {{ product.regular_price.currency }}
                    {{ product.regular_price.value }}
                </span>
                <span class="product-quantity">
                    <button @click="removeProduce(product.selectedOption.product.id)"
                        v-if="product.quantity > 0 && product.selectedOption">-</button>
                    <button @click="removeProduce(product.id)" v-else-if="product.quantity > 0">-</button>
                    {{ product.quantity }}
                    <button @click="addProduct(product.selectedOption.product.id)"
                        v-if="product.selectedOption">-</button>
                    <button @click="addProduct(product.id)" v-else>+</button>
                </span>
                <span class="product-price">
                    {{ product.regular_price.currency }}
                    {{ parseFloat(product.regular_price.value * product.quantity).toFixed(2) }}
                </span>
                <img class="trash-image" src="../assets/vue-test-master/assets/images/trash.png"
                    @click="deleteCartProduct(product, product.quantity)">

            </li>

            <li class="product-card" v-for="product in store.cartItemsObj" v-else>
                <div class="thumbnail-wrapper">
                    <img v-if="product.selectedOption" v-bind:src="`${product.selectedOption.product.image}`"
                        class="product-image">
                    <img v-else v-bind:src="`${product.image}`" class="product-image">
                </div>
                <div class="product-data-wrapper">
                    <span class="product-title">
                        Title: {{ product.title }}
                    </span>
                    <span class="product-brand">
                        Brand: {{ product.brand }}
                    </span>
                    <div v-if="product.selectedOption" class="product-options">
                        <span v-for="value in product.configurable_options[0].values">
                            <span v-if="value.value_index === product.selectedOption.attributes[0].value_index">
                                Label:
                                {{ value.label }}
                            </span>
                        </span>
                        <span v-for="value in product.configurable_options[1].values">
                            <span v-if="value.value_index === product.selectedOption.attributes[1].value_index">
                                Size:
                                {{ value.label }}
                            </span>
                        </span>
                    </div>
                    <span class="product-currency">
                        Price:
                        {{ product.regular_price.currency }}
                        {{ product.regular_price.value }}
                    </span>
                    <span class="product-quantity">
                        Quantity:
                        <button @click="removeProduce(product.selectedOption.product.id)"
                            v-if="product.quantity > 0 && product.selectedOption">-</button>
                        <button @click="removeProduce(product.id)" v-else-if="product.quantity > 0">-</button>
                        {{ product.quantity }}
                        <button @click="addProduct(product.selectedOption.product.id)"
                            v-if="product.selectedOption">-</button>
                        <button @click="addProduct(product.id)" v-else>+</button>
                    </span>
                    <span class="product-price">
                        Total price:
                        {{ product.regular_price.currency }}
                        {{ parseFloat(product.regular_price.value * product.quantity).toFixed(2) }}
                    </span>
                    <button class="remove-from-cart" @click="deleteCartProduct(product.title, product.quantity)">
                        <span>
                            Remove
                        </span>
                        <img class="trash-image" src="../assets/vue-test-master/assets/images/trash.png">
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

.remove-from-cart>span {
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

.product-currency,
.product-quantity,
.product-price {
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

    .product-data-wrapper>span {
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

.price-product-data-column,
.qty-product-data-column,
.total-product-data-column {
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