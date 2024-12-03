<script setup>
import { computed } from 'vue'
import iconRemove from '@/assets/images/icon-remove-item.svg'
import neutralIcon from '@/assets/images/icon-carbon-neutral.svg'

// currency format
function formatCurrency(value) {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(value)
}

const props = defineProps({
  cartData: {
    type: Array,
    default: () => []
  },
  removeArticle: {
    type: Function,
    required: true
  },
  showModal: Function
})

const articles = computed(() => {
  const uniqueArticlesMap = new Map()
  props.cartData.forEach((article) => {
    const key = JSON.stringify(article)
    if (uniqueArticlesMap.has(key)) {
      uniqueArticlesMap.get(key).quantity++
    } else {
      uniqueArticlesMap.set(key, { ...article, quantity: 1 })
    }
  })
  return Array.from(uniqueArticlesMap.values())
})

const numberOfArticles = computed(() => {
  return props.cartData.length
})

const isEmptyCart = computed(() => {
  return props.cartData.length === 0
})

const totalPrice = computed(() => {
  return articles.value.reduce((sum, article) => {
    return sum + article.quantity * article.price
  }, 0)
})

const handleRemove = (product) => {
  props.removeArticle(product)
}

const handleConfirm = () => {
  props.showModal()
}
</script>

<template>
  <div class="cart">
    <h2 class="cart-title">Your Cart ({{ numberOfArticles }})</h2>
    <div v-if="isEmptyCart" class="cart-empty">
      <img alt="impty-cart" src="../assets/images/illustration-empty-cart.svg" />
      <p>your added items will appear here.</p>
    </div>
    <div v-else class="cart-items">
      <div v-for="(article, key) in articles" :key="key">
        <div class="cart-item">
          <div class="cart-item-infos">
            <p class="cart-item-name">{{ article.name }}</p>
            <p class="quantity">
              <span class="cart-item_quantity"> {{ article.quantity }} x </span>
              <span class="item_price"> @{{ formatCurrency(article.price) }} </span> <br />
              <span class="total-price_quantity">{{
                formatCurrency(article.quantity * article.price)
              }}</span>
            </p>
          </div>
             <button
            aria-label="removeBtn"
            class="cart-item_remove-btn"
            @click="handleRemove(article)">
            <img alt="removeIcon" :src="iconRemove" />
            <span class="tooltiptext">Remove item?</span>
          </button>
        </div>
      </div>
      <div class="order-total">
           <p class="order-total_text">Order Total</p>
           <p class="order-total_value">{{ formatCurrency(totalPrice) }}</p>

        
      </div>
      <div class="order_delivery">
          <img class="carbon-nuetral-icon" :src="neutralIcon" alt="carbon-nuetral-icon" />
          <p class="order_delivery-text">
            This is a <span> carbon-nuetral</span> delivery
          </p>
      
      </div>

      <button class="order_delivery-confirmBtn" @click="handleConfirm">Confirm Order</button>
    </div>
  </div>
</template>

<style scoped>
.cart {
  position: relative;
  display: flex;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.5);
  padding: 15px 20px 30px;
  border-radius: 10px;
  justify-content: space-between;
  align-items: center;
  span {
    font-weight: var(--weight-bold);
    margin-left: 2px;
  }
 
}
.cart-empty {
  display: flex;
  flex-direction: column;
  gap: 15px;
  text-align: center;
  align-items: center;
  font-weight: var(--weight-semibold);
}
.cart-title {
  color: rgb(216, 63, 7);
}
.cart-items {
  display: flex;
  margin-top: 25px;
  flex-direction: column;
  gap: 15px;
}
.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 0.1px solid var(--rose-100);
  padding-bottom: 10px;
}
.cart-item-name {
  font-weight: var(--weight-bold);
  font-size: 1.2rem;
  margin-bottom: 10px;
  color: #254e2b;
}
.cart-item_quantity {
  color: rgb(180, 62,  19);
  font-size: 1.2em;
}
.order-total {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-right: 5%;
  margin-top: 20px;
  border-radius: 10px;
  font-weight: var(--weight-bold);
}
.order-total_value {
  color: red;
}
.order_delivery {
  display: flex;
  padding: 20px 15px;
  gap: 0.625rem;
  justify-content: space-between;
  align-items: center;
  background: var(--rose-100);
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.1);
  margin-left: -3%;
  margin-right: auto;
}
.order_delivery-text {
  margin-top: 1rem;
  font-size: .75rem;
}
.carbon-nuetral-icon{
  max-width: min-content;
  margin-top: 1em;
}
.quantity {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 30px;
  border-radius: 10px;
}
.total-price_quantity {
  position: absolute;
  left: 18vw;
  
}
.item_price {
  color: var(--rose-500);
  position: absolute;
  left: 7vw;
}
.cart-item_remove-btn {
  position: relative;
  display: flex;
  border: 1px solid rgba(233, 7, 7, 0.38);
  padding: .2em;
  background: transparent;
  border-radius: 50%;
  align-items: center;
  justify-content: center;
  margin-bottom: 0.5em;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.1);
}
.cart-item_remove-btn .tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;
  position: absolute;
  z-index: 1;
  top: -5px;
  right: 125%;
}
.cart-item_remove-btn .tooltiptext::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 100%;
  margin-top: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: transparent transparent transparent black;
}
.cart-item_remove-btn:hover .tooltiptext {
  visibility: visible;
}
.order_delivery-confirmBtn {
  background-color: var(--rose-100);
  border-radius: 45px;
  color: black;
  border: 2px solid rgb(216, 63, 7);
  padding: 15px;
  font-weight: var(--weight-semibold);
  cursor: pointer;
  min-width: 100%;
  
}
.order_delivery-confirmBtn:hover {
  background-color: rgb(216, 63, 7);
  color: white;
}
button{
  padding: 10px 20px;
  margin-top: 20px;
}
@media (min-width:150px) and (max-width: 299px){
   .cart {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    top: 100%;
    left: -6%;
    margin-top: 20px;
    max-width: max-content;
    margin-right: 2%;
    font-size: small;
  }
  .quantity {
    justify-content: space-between;
    align-items: center;
  }
  .total-price_quantity {
    left: 8em;
  }
  .item_price {
    left: 3.5em;
  }
  .order-total{
    font-size: 1em;
  
  }
  .cart-item-name{
    font-size: small;
  }
  
}
@media (min-width: 300px) and (max-width: 550px){
  .cart {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    top: 100%;
    left: auto;
    margin-top: 20px;
    max-width: max-content;
    margin-left: auto;
    margin-right: auto;
  }
  .quantity {
    justify-content: space-between;
    align-items: center;
  }
  .total-price_quantity {
    left: 11em;
  }
  .item_price {
    left: 5em;
  }
  .order_delivery {
    margin: 0 auto;
  }
 
}

@media(min-width: 551px) and (max-width: 699px) {
  .cart {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    top: 100%;
    left: auto;
    margin-top: 20px;
    max-width: max-content;
    margin-left: auto;
    margin-right: auto;
  }
  .quantity {
    justify-content: space-between;
    align-items: center;
  }
  .total-price_quantity {
    left: 13em;
  }
  .item_price {
    left: 6em;
  }
  .cart-item__remove-btn {
    left: 10em;
  }
  .order_delivery {
    margin: 0 auto;
  }
 
}
@media (min-width: 700px) and (max-width: 799px){
    .cart {
      display: flex;
      flex-direction: column;
      margin-left:auto;
      margin-right:auto;
      left: 71%;
      bottom: 650px;
      max-width: 27vw;
    }
    .quantity {
      justify-content: space-between;
      align-items: center;
      font-size: .80em;
    }
    .cart-item-name {
      font-size: .80em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 0.9em;
      margin-left: 0 auto;
      margin-right: 3%;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    
    
 }
@media (min-width:800px) and (max-width:899px) {
     .cart {
      display: flex;
      flex-direction: column;
      margin-left:auto;
      margin-right:auto;
      left: 71%;
      bottom: 700px;
      max-width: 26vw;
    }
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: .80em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    
}
@media (min-width: 900px) and (max-width: 999px){
  .cart{
    display:flex;
    flex-direction: column;
    max-width: 26vw;
    left: 101%;
    bottom: 750px;
  }
  .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
   
}
@media (min-width: 1000px) and (max-width: 1022px){
   .cart {
      display: flex;
      flex-direction: column;
      left: 101%;
      max-width: 25vw;
      bottom: 800px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
   
}
@media (min-width: 1021px) and (max-width: 1150px){
   .cart {
      display: flex;
      flex-direction: column;
      left: 101%;
      max-width: 25vw;
      bottom: 840px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
    
}
@media (min-width: 1151px) and (max-width: 1190px){
   .cart {
      display: flex;
      flex-direction: column;
      left: 101%;
      max-width: 25vw;
      bottom: 860px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
   
}
@media (min-width: 1191px) and (max-width: 1240px) {
  .cart {
      display: flex;
      flex-direction: column;
      left: 101%;
      max-width: 25vw;
      bottom: 950px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
   
}
@media (min-width: 1241px) and (max-width: 1335px){
  .cart {
      display: flex;
      flex-direction: column;
      left: 102%;
      max-width: 24vw;
      bottom: 950px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }   
}
@media only screen and (min-width: 1326px){
   .cart {
      display: flex;
      flex-direction: column;
      left: 102%;
      max-width: 24vw;
      bottom: 970px;
    } 
    .quantity {
      font-size: .80em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
}
@media (min-width: 1336px) and (max-width: 1445px){
  .cart {
      display: flex;
      flex-direction: column;
      left: 101%;
      max-width: 25vw;
      bottom: 1030px;
    } 
    .quantity {
      font-size: 1em;
      justify-content: space-between;
      align-items: center; 
    }
 
    .cart-item-name {
      font-size: 1em;
    }
    .cart-title {
      font-size: large;
    }
    .order-total {
      font-size: 1.1em;
      margin-right: -5px;
    }
    .tooltiptext {
      font-size: 0.7em;
    }
    .order_delivery{
      margin: 0;
    }
   
}
</style>
