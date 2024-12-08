<script setup>
import SuccessIcon from '../assets/images/icon-order-confirmed.svg'
import { computed } from 'vue'

/* function formatCurrency(value) {
 return new Intl.NumberFormat('fil-PH', {
    style: 'currency', 
    currency: 'PHP',
 }).format(value);
} */

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

const { cartData, show, closeModal, resetQuantity } = defineProps({
  cartData: {
    type: Array,
    default: () => []
  },
  show: true,
  closeModal: Function,
  resetQuantity: Function
})
const handleClose = () => {
  resetQuantity()
  closeModal()
  location.reload() 
}

const articles = computed(() => {
  const uniqueArticlesMap = new Map()
  cartData.forEach((article) => {
    
    const key = JSON.stringify(article)
    
    if (uniqueArticlesMap.has(key)) {
      uniqueArticlesMap.get(key).quantity++
    } else {
      
      uniqueArticlesMap.set(key, { ...article, quantity: 1 })
    }
  })
 
  return Array.from(uniqueArticlesMap.values())
})

const totalPrice = computed(() => {
  return articles.value.reduce((sum, article) => {
    return sum + article.quantity * article.price
  }, 0)
})
</script>

<template>
  <div v-if="show" id="overlay" class="overlay">
    <div class="modal">
      <div class="header-title_sticky">
        <img class="modal_success-icon" :src="SuccessIcon" alt="" />
        <h3 class="modal_title">Order Confirmed</h3>
        <p class="modal_text">We hope you enjoy your food</p>
      </div>

      <div class="modal_items">
        <div class="modal_item" v-for="article in articles" :key="article.id">
          <div class="modal_item-infos">
            <div class="modal_item-infos-left">
              <img class="modal_item-img" :src="resolveUrl(article.image.thumbnail)" alt="" />
              <div class="modal_item-infos">
                <h3>
                  <p class="modal_item-name">{{ article.name }}</p>

                  <span class="modal-item_quantity">{{ article.quantity }}x</span>
                  <span class="modal-item_uniquePrice">@{{ formatCurrency(article.price) }}</span>
                </h3>
              </div>
            </div>

            <div>
              <h2>
                <p>
                  <span class="modal-item_price">{{
                    formatCurrency(article.quantity * article.price)
                    }}</span>
                </p>
              </h2>
            </div>
          </div>
        </div>
        <div class="modal_order-infos">
          <h2>
            <p>Order total</p>
          </h2>
          <h2>
            <p class="modal_total-price">{{ formatCurrency(totalPrice) }}</p>
          </h2>
        </div>
      </div>
      <button class="order_delivery-confirmBtn" id="close-button" @click="handleClose">Start New Order</button>
    </div>
  </div>
</template>
<style scoped>
/* Modal container (hidden by default) */
  .overlay{
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(2px);
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  /* Modal content */
 .modal {
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  text-align: left;
  overflow-y: scroll;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.3s ease-in-out;
 }
 .header-title_sticky{
  position: sticky;
  background-color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  z-index: 1000;
  top: -20px;
 }
 .modal_success-icon{
  position: relative;
  left: 0.40em;
 }
 .modal_title{
  position: relative;
  left: 0.40em;
 }
 .modal_text{
  position: relative;
  left: 0.40em;
 }
 .modal_items{
  background: var(--rose-100);
  padding:20px;
  border-radius: 5px;
  gap: 20px;
  flex-direction: column;
 }
 .modal_item{
  gap:20px;
  border-bottom: 1px solid rgba(238, 226, 226,  0.945);
  padding-bottom: 20px;
 }
 .modal_item-img{
  border-radius: 10px;
  max-width: 50px;
 }
 .modal_item-infos{
  display: flex;
  min-width: 100%;
  justify-content: space-between;
  align-items: center;
 }
 .modal_item-infos-left{
  display: flex;
  gap: 20px;
 }
 .modal-item_quantity{
  margin-right: 25px;
  font-weight: var( --weight-bold);
  color: rgb(180, 62,  19);
 }
 .modal-item_uniquePrice{
  color: var(--rose-500);
 }
 .modal-item_price{
  position: relative;
  top: 0.55em;
  font-weight: var(--weight-bold);
  font-size: 1.25rem;
 }
 .modal_order-infos{
  display: flex;
  min-width: 100%;
  justify-content: space-between;
  align-items: center;
  margin-top: 12px;
 }
 .modal_total-price{
  font-size: 1.5rem;
  font-weight: var(--weight-bold);
  color: red;
 }
 button{
  padding: 10px 20px;
  margin-top: 20px;
  cursor: pointer;
 }
 /*#close-button{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
 }*/
 .order_delivery-confirmBtn{
  background-color: var(--rose-100);
  min-width: 100%;
  border-radius: 45px;
  border: 2px solid  rgb(216, 63, 7);
  color: black;
  padding: 15px;
  font-weight: var(--weight-semibold);
  margin-bottom: 10px;
 
 }
 .order_delivery-confirmBtn:hover{
  background-color: rgb(216, 63, 7);
  color: white;
 }
  /* @-o-keyframes animation*/
 @keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
 }
 @media (min-width: 140px) and (max-width: 149px) {
   .overlay {
     padding: 0;
     margin: 0;
     justify-content: center;
     align-items: center;
     left: -1em;
   }

   .modal {
     padding: 5px;
     font-size: 0.60rem;
   }

   .modal_success-icon {
     max-width: 25vw;
   }

   .modal_title {
     font-size: medium;
   }

   .modal_text {
     font-size: 0.70rem;
   }

   .modal_item-img {
     position: relative;
     left: -12px;
     height: 50px;
     max-width: 40px;
   }

   .modal_item-name {
     position: relative;
     left: -2em;
   }

   .modal-item_quantity {
     position: relative;
     left: -2em;
   }

   .modal-item_uniquePrice {
     position: relative;
     right: 4em;
   }
   .modal-item_price {
     position: relative;
     left: -2em;
     top: 0.55em;
     font-size: small;
   }
   .modal_order-infos {
     position: relative;
     left: -1.2em;
     font-size: 0.95em;
     margin-bottom: 5px;
     margin-top: 10px;
   }
   .modal_total-price {
     position: relative;
     left: 1em;
     font-size: medium;
   }
 }
 @media (min-width: 150px) and (max-width: 199px){
    .overlay {
        padding: 0;
        margin: 0;
        justify-content: center;
        align-items: center;
        left: -1em;
      }
    
      .modal {
        padding: 3px;
        font-size: 0.60rem;
      }
    
      .modal_success-icon {
        max-width: 25vw;
      }
    
      .modal_title {
        font-size: medium;
      }
    
      .modal_text {
        font-size: 0.70rem;
      }
    
      .modal_item-img {
        position: relative;
        left: -12px;
        height: 50px;
        max-width: 40px;
      }
    
      .modal_item-name {
        position: relative;
        left: -2.6em;
      }
    
      .modal-item_quantity {
        position: relative;
        left: -2em;
      }
    
      .modal-item_uniquePrice {
        position: relative;
        right: 4em;
      }
    
      .modal-item_price {
        position: relative;
        left: -2em;
        top: 0.55em;
        font-size: small;
      }
    
      .modal_order-infos {
        position: relative;
        left: -1.2em;
        font-size: 0.95em;
        margin-bottom: 5px;
        margin-top: 10px;
      }
    
      .modal_total-price {
        position: relative;
        left: 1em;
        font-size: medium;
      }
 }
 @media (min-width:200px) and (max-width: 300px) {
  .overlay {
      padding: 0;
      margin: 0;
      justify-content: space-between;
      align-items: center;
      left: -1em;
    }
    .modal {
      padding: 5px;
      font-size: 0.60rem;
    }
  
    .modal_success-icon {
      max-width: 25vw;
    }
  
    .modal_title {
      position: relative;
      font-size: medium;
      left: 0.65em;
    }
  
    .modal_text {
      position: relative;
      font-size: 0.70rem;
      left: 1em;
    }
  
    .modal_item-img {
      position: relative;
      left: -14px;
      height: 50px;
      max-width: 40px;
    }
    .modal_item-name {
      position: relative;
      left: -2.9em;
    }
  
    .modal-item_quantity {
      position: relative;
      left: -2.6em;
    }
  
    .modal-item_uniquePrice {
      position: relative;
      right: 4em;
    }
  
    .modal-item_price {
      position: relative;
      left: -2em;
      top: 0.55em;
      font-size: small;
    }
    .modal_order-infos {
      position: relative;
      left: -1.2em;
      font-size: 0.95em;
      margin-bottom: 5px;
      margin-top: 10px;
      justify-content: space-between;
      align-items: center;
    }
    .modal_total-price {
      font-size: medium;
    }
 }
 @media (min-width: 301px) and (max-width: 499px){
    .overlay {
        padding: 0;
        margin: 0;
        justify-content: space-between;
        align-items: center;
        left: -1em;
      }
      .modal {
        padding: 10px;
        font-size: 1rem;
      }
    
      .modal_item-img {
        position: relative;
        left: -14px;
        height: 4em;
        top: 0.45em;
        
      }
      .modal_item-name {
        position: relative;
        font-size: 0.80em;
        left: -1.5em;
      }
    
      .modal-item_quantity {
        position: relative;
        left: -1.5em;
      }
    
      .modal-item_uniquePrice {
        position: relative;
        right: 2em;
      }
    
      .modal-item_price {
        position: relative;
        left: -1.2em;
        top: 0.45em;
      }
      .modal_order-infos {
        position: relative;
        margin-bottom: 5px;
        margin-top: 10px;
        justify-content: space-between;
        gap: 20px;
        align-items: center;
        font-size: 0.85rem;
      }
 }
</style>
