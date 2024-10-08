<script setup>
import SuccessIcon from "../assets/images/icon-order-confirmed.svg";
import { computed } from "vue";


const resolveUrl = (relativePath) => {
    return new URL(`../assets/images/${relativePath}`, import.meta.url).href;
};

const { cartData, show, closeModal,resetQuantity } = defineProps({
    cartData: {
        type: Array,
        default: () => []
    }, 
    show: Boolean,
    closeModal: Function,
    resetQuantity: Function
})

const handleClose = () => {
    resetQuantity()
    closeModal()
    location.reload(); //or you can try this "(window.location.reload)" onto this page.
}

// This code snippet is creating a computed property named `articles` using Vue's `computed` function.
const articles = computed(() => {
    const uniqueArticlesMap = new Map();
    cartData.forEach(article => {
        // `const key = JSON.stringify(article);` is creating a unique key for each `article` object by
        // converting the `article` object into a JSON string representation. This allows for easy
        // comparison and identification of unique articles based on their properties. The resulting
        // JSON string serves as a unique identifier for each article in the `uniqueArticlesMap` Map
        // object.
        const key = JSON.stringify(article);
       // The code snippet `if (uniqueArticlesMap.has(key)) { uniqueArticlesMap.get(key).quantity++;`
       // is checking if the `uniqueArticlesMap` already contains an entry with the key `key`. If it
       // does, it retrieves the corresponding value object from the map using
       // `uniqueArticlesMap.get(key)` and increments the `quantity` property of that object by 1.
        if (uniqueArticlesMap.has(key)) {
            uniqueArticlesMap.get(key).quantity++;
        } else {
            // `uniqueArticlesMap.set(key, { ...article, quantity: 1 });` is adding a new entry to the
            // `uniqueArticlesMap` Map object.
            uniqueArticlesMap.set(key, { ...article, quantity: 1 });
        }
    });
    // `return Array.from(uniqueArticlesMap.values());` is creating an array of unique article objects
    // by extracting the values from the `uniqueArticlesMap` Map object.
    return Array.from(uniqueArticlesMap.values());
});

const totalPrice = computed(() => {
    return articles.value.reduce((sum, article) => {
        return sum + article.quantity * article.price;
    }, 0);
});
</script>


<template>
  <div v-if="show" class="overlay">
        <div class="modal">
            <img class="modal__success-icon" :src="SuccessIcon" alt="">
            <h3 class="modal__title">Order Confirmed</h3>
            <p class="modal__text">We hope you enjoy your food</p>

            <div class="modal__items">
                <div class="modal__item" v-for="article in articles" :key="article.id">
                    <div class="modal__item-infos">
                        <div class="modal__item-infos-left">
                            <img class="modal__item-img" :src="resolveUrl(article.image.thumbnail)" alt="">
                            <div  class="modal__item-infos">
                                <h3>
                                    <p class="modal__item-name">{{ article.name }}</p>
                                    
                                    <span class="modal-item__quantity">{{ article.quantity }}x</span>
                                   <span class="modal-item__uniquePrice">@${{ article.price }}</span>
                                  
                                    
                                </h3>
                            </div>
                        </div>

                        <div>
                            <h2><p><span class="modal-item__price">${{ article.quantity * article.price }}</span></p></h2>

                        </div>
                    </div>

                </div>
                <div class="modal__order-infos">
                    <p><h2>Order total</h2></p>
                    <h2><p class="modal__total-price"> ${{ totalPrice }}</p></h2>
                </div>
            </div>
            <button class="order__delivery-confirmBtn" @click="handleClose">Start New Order
            </button>
        </div>
    </div>
     
</template>

<style scoped>
.overlay {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    justify-content: center;
    align-items: center;
    max-width: 100%;
    
}

.modal {
    background-color: white;
    padding: 40px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    text-align: left;
    max-height: calc(100vh - 20vh);
    overflow-y: scroll;
    width: 100%;
    height: auto;
    
}
.modal__items {
    box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.2);
    padding: 20px;
    border-radius: 5px;
    display: flex;
    gap: 20px;
    flex-direction: column;
    font-size: 17px;
    justify-content: space-between;
    
}
.modal__title{
    font-size: 2em;
    margin-top: -4px;
   
}
.modal__text{
    font-size: 1em;
    margin-top: -4px;
    margin-bottom: 5px;
}
.modal__item {
    display: flex;
    gap: 20px;
    border-bottom: 1px solid rgba(238, 226, 226, 0.945);
    padding-bottom: 20px;
}

.modal__item-img {
    border-radius: 10px;
    height: auto;
    max-width: 100%;
}
.modal__success-icon{
    margin-top: -20px;
}
.modal__item-infos {
    display: flex;
    width: 100%;
    height: auto;
    justify-content: space-between;
    align-items: center;
   
   

}

.modal__item-infos-left {
    display: flex;
    gap: 10px;
    width: 100%;
    height: auto;
    margin-top: -20px;
}

.modal-item__quantity {
    margin-right: 25px;
    font-weight: var(--weight-bold);
    color: rgb(180, 62, 19);
    font-weight: var(--weigth-bold);

}

.modal-item__uniquePrice {
    color: var(--rose-500);

}

.modal-item__price {
    font-weight: var(--weight-bold);
    font-size: 1.25em;
}
.modal-item__price{
   color: brown;
  }

.modal__order-infos {
    display: flex;
    min-width: 100%;
    justify-content: space-between;
    align-items: center;
    font-weight: var(--weigth-bold);
}

.modal__total-price {
    font-size: 1.30em;
    font-weight: var(--weight-bold);

}

button {
    padding: 10px 20px;
    margin-top: 20px;
    cursor: pointer;
    font-size: 1.50em;
    
}

#open-button {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.order__delivery-confirmBtn {
    background-color: rgb(216, 63, 7);
    height: auto;
    margin-left: auto;
    border-radius: 45px;
    border: none;
    color: var(--rose-100);
    padding: 15px;
    font-weight: var(--weight-semi-bold);
    width: 100%;
}
.order__delivery-confirmBtn:hover{
   box-shadow: 5px 5px 5px rgba(112, 109, 109);
}

@media(max-width:768){
  .modal__item--infos{
    font-size: 60%;
  }
  .modal-item__price{
   color: brown;
   font-size: 60%;
  }
}

@media(max-width:998px){
  .modal__item--infos{
    font-size: 80%;
  }
  .modal-item__price{
   color: brown;
   font-size: 80%;
  }
  .modal {
    width: 100%;
    height: auto;
  }
}
</style>