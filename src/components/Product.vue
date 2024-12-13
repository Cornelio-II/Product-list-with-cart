<script setup>
import cartIcon from '../assets/images/icon-add-to-cart.svg'
import incrementIcon from '@/assets/images/icon-increment-quantity.svg'
import decrementIcon from '@/assets/images/icon-decrement-quantity.svg'
import { ref } from 'vue'

// currency format
function formatCurrency(value) {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(value)
}

const resolveUrl = (relativePath) => {
  return new URL(`../assets/images/${relativePath}`, import.meta.url).href
}
const isSelected = ref(false)
const selectProduct = () => {
  isSelected.value = true
}
const toogleSelectedProduct = () => {
  isSelected.value = !isSelected.value
}
const { addArticle, product, updateQuantity, checkQuantity } = defineProps({
  product: {
    type: Object,
    default: () => {}
  },
  addArticle: Function,
  updateQuantity: Function,
  checkQuantity: Function
})

const handleClick = () => {
  selectProduct()
  addArticle(product)
}
const handleUpdate = () => {
  updateQuantity(product, '-')
}
</script>

<template>
  <div class="product">
    <div class="product_header">
      <picture :class="isSelected ? 'selected' : null" :onclick="toogleSelectedProduct">
        <source
          :srcset="resolveUrl(product.image.desktop)"
          media="(min-width: 120px)"
          type="image desktop/jpg"
        />
        <source
          :srcset="resolveUrl(product.image.tablet)"
          media="(max-width: 768px"
          type="image tablet/jpg"
        />
        <source
          :srcset="resolveUrl(product.image.mobile)"
          media="(max-width: 480px)"
          type="mobile image/jpg"
        />
        <img :src="resolveUrl(product.image.mobile)" :alt="product.name" />
      </picture>
      <button
        aria-label="add-to-cart-btn"
        v-if="!isSelected"
        class="product__header-btn"
        :onclick="handleClick"
      >
        <img :src="cartIcon" alt="Add to cart" />Add to Cart
      </button>
      <!---------show-button------------>
      <button role="show icon button" v-else class="product__header-btn orange">
        <img alt="decrementIcon" :src="decrementIcon" :onclick="handleUpdate" class="img-icon" />{{
          checkQuantity(product)
        }}
        <img alt="incrementIcon" :src="incrementIcon" :onclick="handleClick" class="img-icon" />
      </button>
    </div>
    <!---------div product-header------------>
    <div role="infos items banner" class="product__infos">
      <p role="category item(s) banner" class="product__infos-category">{{ product.category }}</p>
      <p role="name of item(s) banner" class="product__infos-name">{{ product.name }}</p>
      <p role="price of item(s) banner" class="product__infos-price">
        {{ formatCurrency(product.price) }}
      </p>
    </div>
  </div>
  <!---------div product------------>
</template>
<style scoped>
.product {
  display: flex;
  flex-direction: column;
  gap: 25p
}
.product_header {
  position: relative;

}
 img{
    border-radius: 10px;
}
div.product {
  border-radius: 10px;
  box-shadow:
    0 4px 8px 0 rgba(0, 0, 0, 0.2),
    0 6px 20px 0 rgba(0, 0, 0, 0.19);
 
}
div.product:hover {
  border: 1px solid #777;
}
.selected {
  outline: 2px solid red;
  border-radius: 10px;
}

.product__header-btn {
  position: absolute;
  justify-content: center;
  align-items: center;
  top: 100%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 20px;
  padding: 10px 20px;
  border: 1px solid gray;
  font-size: 13px;
  font-weight: var(--weight-semibold);
  background: white;
  width: 75%;
  display: flex;
  gap: 10px;
}

.orange {
  background: rgb(236, 70, 10);
  justify-content: space-between;
}
.product__infos {
  padding: 20px;
}
.product__infos-category{
  color: rgba(15, 44, 2, 0.732);
}
.product__infos-price{
  color: red;
}
@media (min-width: 150px) and (max-width: 299px){
  .product{
    max-width: max-content;
    justify-content: space-between; 
    place-content: center; 
     p {
    font-size: .6em;
  }
  .product__header-btn {
    min-width: max-content;
  }
  .product__infos {
    min-width: max-content;
  }
    
  }
}
@media(min-width: 300px) and (max-width: 550px) {
 .product{
  max-width: max-content;
  }
}
@media (min-width: 700px)  {
  .product{
    max-width: max-content;
  }
  p {
    font-size: 0.80em;
  }
  .product__header-btn {
    min-width: max-content;
  }
  .product__infos {
    min-width: max-content;
  }
}
@media (min-width: 850px){
   p {
    font-size: 0.75em;
  }
  .product__header-btn {
    min-width: max-content;
  }
  .product__infos {
    min-width: max-content;
  }
}
@media (min-width: 1020px) {
  p {
    font-size: 1em;
  }
  .product__header-btn {
    min-width: max-content;
  }

  .product__infos {
    min-width: max-content;
  }
}
</style>
