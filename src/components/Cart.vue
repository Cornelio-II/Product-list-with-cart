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
              <span class="cart-item_quantity"> {{ article.quantity }} x </span>
              <span class="item_price"> @{{ formatCurrency(article.price) }} </span> <br />
              <span class="total-price_quantity">{{
                formatCurrency(article.quantity * article.price)
              }}</span>
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
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.5);
  padding: 15px;
  border-radius: 10px;
  span {
    font-weight: var(--weight-bold);
  }
}
.cart-empty {
  display: flex;
  flex-direction: column;
  gap: 15px;
  text-align: center;
  align-items: center;
  font-weight: var(--weight-semibold);
  height: min-content;

}
.cart-title {
  color: rgb(216, 63, 7);
  text-align: center;
}
.cart-items {
  display: flex;
  margin-top: 25px;
  flex-direction: column;
  gap: 15px;
}
.cart-item {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 0.1px solid var(--rose-100);
  padding-bottom: 10px 20px;
}
.cart-item-infos{
  justify-content: space-between;
  align-items: center;
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
.total-price_quantity {
  left: 8vw;
}
.item_price {
  color: var(--rose-500);
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
@media screen and (min-width:150px){
  .cart {
    max-width: max-content;
    font-size: 0.70em;
  }
  .cart-item_quantity {
    position: relative;
    bottom: 10px;
  }
  .total-price_quantity {
    position: absolute;
    display: flex;
    flex-direction: column;
    text-align: center;
    left: 8em;
    bottom: 11px;
  }
  .item_price {
    position: relative;
    left: 1em;
    bottom: 10px;
  }
  .order-total{
    font-size: 1.1em;
  }
  .cart-item-name{
    font-size: small;
  }
  .cart-item_remove-btn{
    position: relative;
    top: -35px;
  }
  .carbon-nuetral-icon{
    position: relative;
    bottom: 10px;
  }
  .order_delivery-text{
    position: relative;
    font-size: 0.80em;
    bottom: 10px;
  }
}
@media screen and (min-width: 251px)  {
  .cart {
    max-width: max-content;
    font-size: small;
  }
  .cart-item_quantity{
   position: relative;
   bottom: 10px;
  }
  .total-price_quantity {
    position: absolute;
    display: flex;
    flex-direction: column;
    text-align: center;
    left: 11em;
    bottom: 10px;
  }
  .item_price {
    position: relative;
    left: 2em;
    bottom: 10px;
  }
  .order-total {
    font-size: 1.3em;
  }
  .cart-item-name {
    position: relative;
    bottom: 10px;
    font-size: 1em;
  }
  .cart-item_remove-btn {
    position: relative;
    bottom: 35px;
    left: 5%;
  }
}
@media screen and (min-width: 300px) {
  .cart {
      max-width: max-content;
      font-size: 1em;
    }
    .cart-item_quantity {
      position: relative;
      bottom: 10px;
    }
    .total-price_quantity {
      position: absolute;
      display: flex;
      flex-direction: column;
      text-align: center;
      left: 11em;
      bottom: 10px;
    }
    .item_price {
      position: relative;
      left: 2em;
      bottom: 10px;
    }
    .order-total {
      font-size: 1.3em;
    }
    .cart-item-name {
      position: relative;
      bottom: 10px;
      font-size: 1em;
    }
    .cart-item_remove-btn {
      position: relative;
      bottom: 35px;
      left: 5%;
    }
    .carbon-nuetral-icon{
      position: relative;
      bottom: 10px;
    }
    .order_delivery-text{
      position: relative;
      bottom: 10px;
    }
}
@media screen and (min-width: 551px) {
.cart {
    max-width: max-content;
    font-size: 1em;
  }
  .cart-item_quantity {
    position: relative;
    bottom: 10px;
  }
  .total-price_quantity {
    position: absolute;
    display: flex;
    flex-direction: column;
    text-align: center;
    left: 11em;
    bottom: 10px;
  }
  .item_price {
    position: relative;
    left: 2em;
    bottom: 10px;
  }
  .order-total {
    font-size: 1.3em;
  }
  .cart-item-name {
    position: relative;
    bottom: 10px;
    font-size: 1em;
  }
  .cart-item_remove-btn {
    position: relative;
    bottom: 35px;
    left: 5%;
  }
  .carbon-nuetral-icon {
    position: relative;
    bottom: 10px;
  }
  .order_delivery-text {
    position: relative;
    bottom: 10px;
  }
}
@media screen and (min-width: 700px){
  .cart {
      max-width: max-content;
      font-size: 1em;
    }
    .cart-item_quantity {
      position: relative;
      bottom: 10px;
    }
    .total-price_quantity {
      position: absolute;
      display: flex;
      flex-direction: column;
      text-align: center;
      left: 11em;
      bottom: 10px;
    }
    .item_price {
      position: relative;
      left: 2em;
      bottom: 10px;
    }
    .order-total {
      font-size: 1.3em;
    }
    .cart-item-name {
      position: relative;
      bottom: 10px;
      font-size: 1em;
    }
    .cart-item_remove-btn {
      position: relative;
      bottom: 35px;
      left: 5%;
    }
    .carbon-nuetral-icon {
      position: relative;
      bottom: 10px;
    }
    .order_delivery-text {
      position: relative;
      bottom: 10px;
    }
 }
@media screen and (min-width:850px){
.cart {
  font-size: small;
  place-items: center;
  align-items: center;
  justify-content: space-between;
}
.cart-item_quantity {
    position: relative;
    bottom: 10px;
}
.total-price_quantity {
    position: absolute;
    display: flex;
    flex-direction: column;
    text-align: center;
    left: 9em;
    bottom: 10px;
}
.item_price {
    position: relative;
    left: 1em;
    bottom: 10px;
}
.order-total {
    font-size: 1.1em;
 }
.cart-item-name {
    position: relative;
    bottom: 10px;
    font-size: 1em;
}
.cart-item_remove-btn {
    position: relative;
    bottom: 35px;
    left: 5%;
}
.carbon-nuetral-icon {
    position: relative;
    bottom: 10px;
}
.order_delivery-text {
    position: relative;
    bottom: 10px;
  }
}
</style>
