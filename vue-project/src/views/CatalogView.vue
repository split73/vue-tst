<script>
import confProductsJSON from "../assets/vue-test-master/assets/level3/products.json"
import logo from "../assets/vue-test-master/assets/images/logo.png"
import { store } from "../store/store.js"

export default {
  data() {
    return {
      confProducts: confProductsJSON,
      logo: logo,
      clicked: false,
      filterSet: new Set(),
      store: store,
    }
  },
  methods: {
    addBrandToFilter(brand) {
      if (this.filterSet.has(brand)) {
        this.filterSet.delete(brand)
      } else {
        this.filterSet.add(brand)
      }
      return this.filterSet.has(brand)
    },
    addToCart(product) {
      if (product.type === "configurable") {
        if (product.selectedOption) {
          if (Object.hasOwn(this.store.cartItemsObj, product.selectedOption.product.id)) {
            this.store.cartItemsObj[product.selectedOption.product.id].quantity++
          } else {
            this.store.cartItemsObj[product.selectedOption.product.id] = JSON.parse(JSON.stringify(product))
            this.store.cartItemsObj[product.selectedOption.product.id].quantity = 1
          }
        } else {
          return
        }
      }


      else if (Object.hasOwn(this.store.cartItemsObj, product.id)) {
        this.store.cartItemsObj[product.id].quantity++
      } else {
        this.store.cartItemsObj[product.id] = product
        this.store.cartItemsObj[product.id].quantity = 1
      }
      this.store.cartAmount++
    }, showAllowedSizes(product, value) {
      product.selectedColor = value
      product.allowedOptions = [];
      product.variants.map((variant) => {
        if (variant.attributes[0].value_index === product.selectedColor.value_index) {
          product.allowedOptions.push(variant)
        }
      })
    }, selectSize(product, selectedOption) {
      product.selectedOption = selectedOption
    }


  },
  computed: {
    confFilteredByBrand() {


      if (this.filterSet.size === 0) {
        return this.confProducts
      }

      return this.confProducts.filter((product) =>
        this.filterSet.has(product.brand)
      )
    }, filterOptions() {
      this.filterOptionsSet = new Set()
      this.confProducts.map((product) => {
        this.filterOptionsSet.add(product.brand)
      })
      return Array.from(this.filterOptionsSet)
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
      <ul id="filters-wrapper">
        <li v-for="brand in filterOptions" :class="{ filter: true, activeFilter: this.filterSet.has(brand) }">
          <button v-on:click="addBrandToFilter(brand)" class="filter-selector">
            brand {{ brand }}
          </button>
        </li>
      </ul>
    </aside>
    <ul id="product-container">
      <li class="product-card" v-for="product in confFilteredByBrand">
        <div class="thumbnail-wrapper" @click="addToCart(product)">
          <img v-if="product.selectedOption" v-bind:src="`${product.selectedOption.product.image}`"
            class="product-image" draggable="false">
          <img v-else v-bind:src="`${product.image}`" class="product-image" draggable="false">
        </div>
        <div class="product-details">
          <span class="product-title">
            {{ product.title }}
          </span>
          <span class="product-brand">
            Brand: {{ product.brand }}
          </span>
          <span class="product-price">
            {{ product.regular_price.currency }}
            {{ product.regular_price.value }}
          </span>
          <div v-if="product.type === `configurable`">
            <div v-for="specs in product.configurable_options">
              <button class="color-attribute" v-if="specs.attribute_code === 'color'" v-for="values in specs.values"
                :style="{ backgroundColor: values.value }" @click="showAllowedSizes(product, values)">
              </button>
              <button class="size-attribute" v-if="specs.attribute_code === 'size'"
                v-for="allowedOption in product?.allowedOptions" :style="{ background: 'none' }"
                @click="selectSize(product, allowedOption)">
                <span v-for="value in product.configurable_options[1].values">
                  <span v-if="value.value_index === allowedOption?.attributes[1]?.value_index">
                    {{ value.label }}
                  </span>
                </span>
              </button>
            </div>
          </div>
        </div>
      </li>
    </ul>

  </main>
</template>

<style scoped>
.product-card button.color-attribute {
  height: 20px;
  width: 20px;
  margin: 6px 2px;
}

li,
span,
button {
  list-style: none;
  font-size: 18px;
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

#sidebar-filters {
  display: flex;
  flex-direction: column;
  width: 250px;
}

#sidebar-filters li {
  margin-top: 10px;
}

.filter-selector {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  font-size: inherit;
}

#filters-wrapper {
  margin: 0;
  padding: 0;
  border-right: 2px solid black;
}

#product-container {
  display: flex;
  flex-direction: row;
  width: calc(100% - 290px);
  flex-wrap: wrap;
  gap: 10px;
}

.product-image {
  max-height: 600px;
  width: 100%;
  cursor: pointer;
}

.product-details {
  display: flex;
  flex-direction: column;
  width: 100%;
  text-align: center;
}

.color-attribute {
  width: 20%;
  height: 15px;
}

@media (max-width: 700px) {
  #sidebar-filters {
    width: 100%;
    border-bottom: 2px solid #333;
    padding-bottom: 10px;
  }

  #sidebar-filters ul {
    border: 0;
    display: flex;
    flex-direction: row;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
  }

  #filters-title {
    width: 100%;
    text-align: center;
  }

  #product-container {
    width: 100%;
    padding: 10px;
  }

  .color-attribute {
    width: 20px;
    height: 20px;
    margin: 5px 2px;
  }
}

@media (min-width: 805px) {
  .product-card {
    display: flex;
    flex-direction: column;
    width: calc(33.3% - 10px);
    align-items: center;
    min-width: 200px;
  }

  li.product-card:hover {
    transform: translateY(-9px);
    box-shadow: 1px 9px 3px 2px #eee;
  }

  .product-card[data-v-e2844fbc] {
    transition: .2s ease;
    box-shadow: 1px 1px 3px 2px #eee;
  }
}

@media (max-width: 805px) {
  .product-card {
    width: calc(50% - 10px);
  }
}
</style>
