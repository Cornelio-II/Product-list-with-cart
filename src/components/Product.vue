<template>
  <div class="product">
    <div class="product__header">
      <picture id="element"
        class="product__header-img"
        :class="isSelected ? 'selected' : null"
        :onclick="toogleSelectedProduct">
        <source
          :srcset="resolveUrl(product.image.desktop)"
          media="(min-width: 1200px)"/>
        <source
          :srcset="resolveUrl(product.image.tablet)"
          media="(min-width: 768px)"/>
        <source
          :srcset="resolveUrl(product.image.mobile)"
          media="(min-width: 480px)"/>
        <img :src="resolveUrl(product.image.mobile)" :alt="product.name"/>
      </picture>
      <button aria-label="add-to-cart-btn"
        v-if="!isSelected"
        class="product__header-btn"
        :onclick="handleClick">
        <img :src="cartIcon" alt="" />Add To Cart
      </button>
      <button v-else class="product__header-btn  orange">
        <img alt="decrementIcon":src="decrementIcon" :onclick="handleUpdate" class="img-icon"/>{{
          checkQuantity(product)
        }}
        <img alt="incrementIcon" :src="incrementIcon" :onclick="handleClick" class="img-icon"/>
      </button>
    </div>
    <div class="product__infos">
      <p class="product__infos-category"><h3>{{ product.category }}</h3></p>
      <p class="product__infos-name"><h3>{{ product.name }}</h3></p>
      <p class="product__infos-price"><h3>${{ product.price }}</h3></p>
    </div>
  </div>
</template>

<script setup>
import cartIcon from "../assets/images/icon-add-to-cart.svg";
import incrementIcon from "@/assets/images/icon-increment-quantity.svg";
import decrementIcon from "@/assets/images/icon-decrement-quantity.svg";
import { ref } from "vue";


const resolveUrl = (relativePath) => {
  return new URL(`../assets/images/${relativePath}`, import.meta.url).href;
};
const isSelected = ref(false);
const selectProduct = () => {
  isSelected.value = true;
};
const toogleSelectedProduct = () => {
  isSelected.value = !isSelected.value;
};
const { addArticle, product, updateQuantity, checkQuantity } = defineProps({
  product: {
    type: Object,
    default: () => {},
  },
  addArticle: Function,
  updateQuantity: Function,
  checkQuantity: Function,
});

const handleClick = () => {
  selectProduct();
  addArticle(product);
};
const handleUpdate = () => {
  updateQuantity(product, "-");
};
</script>

<style scoped>
.product {
  display: flex;
  flex-direction: column;
  gap: 25px;
  
}

.product__header {
  position: relative;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.6);
}

.product__header-img {
  border-radius: 5px;
  overflow: hidden;
 
}

.img-icon{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  border: 1px solid hsl(20, 50%, 98%);
  width: 20px;
  height: 20px;
  border-radius: 50%;
  transform: scale(0.95);
}
.img-icon:hover{
  color: hsl(14, 86%, 42%);
  border: 1px solid hsl(14, 86%, 42%);
}


#element:hover{
  box-shadow:  12px 12px 12px rgb(112, 109, 109) , -10px -10px 10px rgb(112, 109, 109);
  ;
}

.selected {
  outline: 2px solid rgb(229, 3, 3);
}
 .product__header-btn {
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 25px;
    padding: 10px 20px;
    border: 1px solid gray;
    font-weight: var(--weight-bold);
    background: white;
    width: 80%;
    display: flex;
    gap: 10px;
    
    }
    
.orange {
  background: rgb(236, 70, 10);
  justify-content: space-between;
 

}
.orange:hover {
  box-shadow: 5px 5px 5px rgba(112, 109, 109);
}   
.product__infos {
    font-size: 1.30em;
    margin-top: 25px;
    margin-bottom: 10px;
    }
    .product__infos-price {
    color: #eb0d0de4;
    }

.product__infos-category{
  color: rgb(18, 211, 114);
}
@media(max-width:998px){
  .product__header-btn {
    font-size: 80%;
  }

}
</style>
