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
      allowedSizes: new Set()
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
    },
    confButtonVariants(variants) {
      variants.map(((el) => {
        console.log(el)
      }))
      return variants
    },
    confButtonSize(product, selectderColor = 0 ){
      if (selectderColor === 0){
        this.allowedSizes = new Set()
        product.variants.map((el) => {
          el.attributes.map((nestedEl) => {
            if (nestedEl.code === "size"){
              this.allowedSizes.add(nestedEl.value_index)
            }
          })
        })
        
        return this.allowedSizes
      } else {
        this.allowedSizes = new Set()
        product.variants.map((el) => {
          if (el.attributes[0].value_index === selectderColor){
            this.allowedSizes.add(el.attributes[1].value_index)
          }
        })
      }
      console.log("RES", Array.from(this.allowedSizes))
      return Array.from(this.allowedSizes)
    }
    
  },
  computed: {
    confFilteredByBrand() {
      if (this.filterSet.size === 0){
        return this.confProducts
      }

      return this.confProducts.filter((product) => 
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
      <ul id="filters-wrapper" >
        <li v-for="product in confProducts" :class="{ filter: true, activeFilter: this.filterSet.has(product?.brand) }">
          <button 
            v-on:click ="addBrandToFilter(product)"
            class="filter-selector">
            brand {{ product.brand }}
          </button>
        </li>
      </ul>
    </aside>

  <ul id="product-container"  >
    <li class="product-card" v-for="product in confFilteredByBrand" >
      <div class="thumbnail-wrapper" @click="addToCart(product)">
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
        <div v-if="product.type === `configurable`">
          

          <div v-for="specs in product.configurable_options">
            <button class="color-attribute" v-if="specs.attribute_code === 'color'" v-for="values in specs.values" :style="{ backgroundColor: values.value }">
            </button>
            <button class="size-attribute" v-if="specs.attribute_code === 'size'" v-for="values in specs.values" :style="{ background: 'none' }">
              {{values.label}}
              {{  this.confButtonSize(product) }}
            </button>
          </div>



          <!-- <div v-for="specs in product.configurable_options">
            <button class="color-attribute" v-if="specs.attribute_code === 'color'" v-for="values in specs.values" :style="{ backgroundColor: values.value }">
            </button>
            <button class="size-attribute" v-if="specs.attribute_code === 'size'" v-for="values in specs.values" :style="{ background: 'none' }">
              {{values.label}}
            </button>
          </div> -->
          
        </div>
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
  border-right: 2px solid black;
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
</style>
