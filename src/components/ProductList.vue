`<script setup>
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
     <h1 class="main-title-sticky">Desserts</h1>
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
.main-title-sticky{
 position:sticky;
 top:0;
 margin-top: -20px;
 margin-bottom: 12px;
 border: none;
 text-align: center;
 background-color: var(--rose-50);
 box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.3);
 padding: 10px 20px;
 z-index: 1000;
 align-items: center;
 justify-content: center;
}
.product_container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
@media(min-width: 180px) and (max-width: 550px) {
  .product_container{
    display: grid;
    grid-template-columns: 1fr;
  }
}
@media(min-width: 551px) and (max-width: 699px){
  .product_container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}
@media  (min-width: 700px) and (max-width:  799px){
 .product_container{
  display: grid;
  grid-template-columns: repeat(3,  1fr);
  margin: 3px;
 }
}
@media (min-width: 800px) and (max-width: 999px){
     .product_container{
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
      margin-left: -20px;
      margin-right: auto;
   
  }
}
@media (min-width: 1000px) and (max-width: 1999px){
  .product_container{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
  }
}
</style>
`