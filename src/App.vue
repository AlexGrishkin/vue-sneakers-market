<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'

import Drawer from './components/Drawer.vue'

const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

// const filters = reactive({
//   sortBy: 'title',
//   searchQuery: ''
// })

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(`https://c0abc61e8e424c50.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value
    })

    cart.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

// const onClickAddPlus = (item) => {
//   if (!item.isAdded) {
//     addToCart(item)
//   } else {
//     removeFromCart(item)
//   }
//   console.log(cart)
// }

// const onChangeSelect = (event) => {
//   filters.sortBy = event.target.value
// }

// const onChangeSearchInput = (event) => {
//   filters.searchQuery = event.target.value
// }

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://c0abc61e8e424c50.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

// const addToFavorite = async (item) => {
//   try {
//     if (!item.isFavorite) {
//       const obj = {
//         parentId: item.id
//       }

//       item.isFavorite = true

//       const { data } = await axios.post('https://c0abc61e8e424c50.mokky.dev/favorites', obj)

//       item.favoriteId = data.id

//       console.log(data)
//     } else {
//       item.isFavorite = false

//       await axios.delete(`https://c0abc61e8e424c50.mokky.dev/favorites/${item.favoriteId}`)

//       item.favoriteId = null
//     }
//   } catch (err) {
//     console.log(err)
//   }
// }

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://c0abc61e8e424c50.mokky.dev/items', { params })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', { cart, closeDrawer, openDrawer, addToCart, removeFromCart })
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :button-disabled="cartButtonDisabled"
  />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10"></div>
  </div>
</template>

<style scoped></style>
