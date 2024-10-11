<script setup>
import { computed } from "vue";
import iconRemove from "@/assets/images/icon-remove-item.svg";
import neutralIcon from "@/assets/images/icon-carbon-neutral.svg";

const props = defineProps({
  cartData: {
    type: Array,
    default: () => [],
  },
  removeArticle: {
    type: Function,
    required: true,
  },
  showModal: Function,
});

// This code block is creating a computed property named `articles` that processes the `cartData` array
// to group articles by their unique properties. It uses a `Map` to keep track of unique articles based
// on a stringified version of each article object.
const articles = computed(() => {
  const uniqueArticlesMap = new Map();
  props.cartData.forEach((article) => {
    const key = JSON.stringify(article);
    if (uniqueArticlesMap.has(key)) {
      uniqueArticlesMap.get(key).quantity++;
    } else {
      uniqueArticlesMap.set(key, { ...article, quantity: 1 });
    }
  });
  return Array.from(uniqueArticlesMap.values());
});

// The `numberOfArticles` constant is creating a computed property using Vue's `computed` function. It
// calculates the number of articles in the `cartData` array by returning the length of the `cartData`
// array passed as a prop to the component. This computed property will automatically update whenever
// the `cartData` array changes, ensuring that the number of articles displayed in the cart is always
// up to date.
const numberOfArticles = computed(() => {
  return props.cartData.length;
});

const isEmptyCart = computed(() => {
  return props.cartData.length === 0;
});

// The `const totalPrice` is a computed property that calculates the total price of all articles in the
// cart. It uses the `articles` computed property, which is an array of unique articles with
// quantities, to iterate over each article and calculate the total price by multiplying the quantity
// of each article with its price and summing them up using the `reduce` method. This ensures that the
// total price is dynamically updated whenever the cart data changes.
const totalPrice = computed(() => {
  return articles.value.reduce((sum, article) => {
    return sum + article.quantity * article.price;
  }, 0);
});

const handleRemove = (product) => {
  props.removeArticle(product);
};

const handleConfirm = () => {
  props.showModal();
};
</script>

<template>
  <div class="cart">
    <h5 class="cart__title">Your Cart ({{ numberOfArticles }})</h5>
    <div v-if="isEmptyCart" class="cart-empty">
      <img src="../assets/images/illustration-empty-cart.svg" alt="" />

      <p>your added items will appear here.</p>
    </div>
    <div v-else class="cart-items">
      <div v-for="(article, key) in articles" :key="key">
        <div class="cart-item">
          <div class="cart-item__infos">
            <p class="cart-item__name">{{ article.name }}</p>
            <p>
              <span class="cart-item__quantity">
                {{ article.quantity }} x
              </span>
              <span> @ ${{ article.price }} </span>
              <span>${{ article.quantity * article.price }}</span>
            </p>
          </div>
          <button class="cart-item__remove-btn" @click="handleRemove(article)">
             <img :src="iconRemove" alt=""  style="height: fit-content;"/>
          </button>
        </div>
      </div>
      <div class="order-total">
        <h3><p class="order-total__text">Order Total</p></h3>
        <h3><p class="order-total__value">${{ totalPrice }}</p></h3>
      </div>
      <div class="order__delivery">
        <div class="order__delivery-icon">
          <img class="" :src="neutralIcon" alt="" />
          <p class="order__delivery-text">
            This is a <span class="bold"> carbon-nuetral</span> delivery
          </p>
        </div>
      </div>

      <button class="order__delivery-confirmBtn"  @click="handleConfirm">Confirm Order
        
      </button>
    </div>
  </div>
</template>

<style scoped>
.cart {
  padding: 2em;
  border-radius: .3em;
  margin-top: 30px;
  height: fit-content;
  width: 100%;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.9);
  span {
    font-weight: bold;
    position: relative;
    margin-left: 2px;
  }
}
.cart-empty {
  display: flex;
  flex-direction: column;
  gap: 15px;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: min-content;
  font-size: 1.3rem;
}

.cart__title {
  font-size: 2rem;
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
  font-size: 1.5rem;
}

.cart-item__name {
  font-weight: var(--weigth-bold);
  font-size: 1.5rem;
  margin-bottom: 10px;
 

}

.cart-item__remove-btn {
  border: 1px solid rgb(14, 225, 53);
  background: transparent;
  border-radius: 50%;
  display: block;
  align-items: center;
  padding: 1rem;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.1);
  

}
.cart-item__quantity {
  color: rgb(216, 63, 7);
  font-weight: var(--weigth-bold);
  font-size: 1.rem;
}
.order-total {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 25px;
  border-radius: 10px;
  font-size: 1.4em;
  
}
.order-total__value {
  font-size: 2.1rem;
  font-weight: var(--weigth-bold);
 
}

.order__delivery {
  padding: 20px 15px;
  display: flex;
  font-size: 20px;
  border-radius: 10px;
  gap: 10px;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.1);
}

.order__delivery-confirmBtn {
  background-color: rgb(216, 63, 7);
  border-radius: 20px;
  border: none;
  color: var(--rose-100);
  padding: 2px;
  font-weight: var(--weigth-bold);
  cursor: pointer;
  font-size: 1.60em;
  width: 100%;
  height: auto;
  margin-left: auto;
  
}
.order__delivery-confirmBtn:hover {
  box-shadow: 5px 5px 5px rgba(112, 109, 109);
}

@media(max-width:998px){
    .order__delivery-confirmBtn {
      font-size: 82%;
      background-color: rgb(216, 63, 7);
      border-radius: 20px;
      padding: 5px 20px;
    }
  .order-total__text {
    font-size: 38.2%;
    margin-right: 8px;
    
  }
    .order-total__value{
      font-size: 30px;
      color: brown;
  }
 
}
</style>
