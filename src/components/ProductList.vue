<script setup>
import Product from './Product.vue'

const { addArticle, products, updateQuantity } = defineProps({
  products: Object,
  addArticle: Function,
  updateQuantity: Function,
  checkQuantity: Function
})
</script>

<template>
  <div class="productlist">
    <div class="product_container">
      <div v-for="product in products" :key="product.id">
        <Product
          :product="product"
          :addArticle="addArticle"
          :updateQuantity="updateQuantity"
          :checkQuantity="checkQuantity"
        />
      </div>
    </div>
  </div>
</template>

<style scoped> 
.product_container {
  gap: 16px;
}
@media screen and (min-width: 150px) {
  .product_container{
    display: grid;
    grid-template-columns: 1fr;
  }
}
@media screen and (min-width: 501px){
  .product_container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}
@media  screen and (min-width: 700px){
 .product_container{
  display: grid;
  grid-template-columns: repeat(3,  1fr);
 
 }
}
@media screen and (min-width: 850px){
    .productlist{
      grid-column: span 3;
    }
     .product_container{
      display: grid;
      grid-template-columns: repeat(3, 1fr);
  }
}
</style>