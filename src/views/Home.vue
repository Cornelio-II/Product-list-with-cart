<script setup>
import products from '../data/data.json'
import ProductList from '../components/ProductList.vue'
import Cart from '../components/Cart.vue'
import Modal from '../components/Modal.vue'
import { ref } from 'vue'

const cartData = ref([])

const addArticle = (product) => {
  cartData.value.push(product)
}

const removeArticle = (product) => {
  cartData.value = cartData.value.filter((article) => article.name !== product.name)
}

const updateQuantity = (product) => {
  const index = cartData.value.findIndex((item) => item.name === product.name)
  if (index !== -1) {
    cartData.value.splice(index, 1)
  }
}

const checkQuantity = (product) => {
  return cartData.value.filter((item) => item.name === product.name).length
}

const resetQuantity = () => {
  cartData.value = []
}

const show = ref(false)

const showModal = () => {
  show.value = true
}

const closeModal = () => {
  show.value = true
}
</script>

<template>
      <ProductList
        :products="products"
        :cartData="cartData"
        :addArticle="addArticle"
        :updateQuantity="updateQuantity"
        :checkQuantity="checkQuantity"/>
     
      <Cart :cartData="cartData" :removeArticle="removeArticle" :showModal="showModal" />
  
      <Modal
        :cartData="cartData"
        :showModal="showModal"
        :closeModal="closeModal"
        :show="show"
        :resetQuantity="resetQuantity"/>
   
</template>
