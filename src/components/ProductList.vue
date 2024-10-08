<script setup>
import Product from "./Product.vue";

const { addArticle, products, updateQuantity } = defineProps({
  products: Object,
  addArticle: Function,
  updateQuantity: Function,
  checkQuantity: Function
})
</script>

<template>
  <div class="productList">
    <div style="font-size: 25px; text-align: center;">
      <h1>Desserts</h1>
    </div>
    <div class="products__container">
      <div v-for="product in products" :key="product.id">
        <Product
          :product="product"
          :addArticle="addArticle"
          :updateQuantity="updateQuantity"
          :checkQuantity="checkQuantity"/>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  margin-bottom: 50px;
  
}
 .products__container {
    display: flex;
    flex-direction: column;
    grid-gap: 2em;
}

/* */

/* tablettes */
@media (min-width : 376px) and (max-width: 850px){
    .productList {
        grid-column: span 3;
    }
    .products__container {
        display: grid;
        grid-template-columns: 1fr;
        
    }
}

@media screen and (min-width: 550px) {
    .productList {
        grid-column: span 3;
    }
    .products__container {
        display: grid;
        grid-template-columns: 1fr 1fr;
        
    }
}

@media screen and (min-width: 700px) {
    .productList {
        grid-column: span 3;
    }
    .products__container {
        display: grid;
        grid-template-columns: 1fr 1fr ;  
    }
}
@media (min-width: 950px) {
    .productList {
        grid-column: span 3;
    }
    .products__container {
        display: grid;
        grid-template-columns: 1fr  1fr 1fr;  
    }
}
@media screen and (max-width: 120px) {
    .productList {
        grid-column: span 3;
    }
    .products__container {
        display: grid;
        grid-template-columns: 1fr;   
    }
}
</style>