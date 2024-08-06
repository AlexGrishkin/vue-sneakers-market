<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavorite'])

const onClickAdd = () => {
  alert('Добавить')
}

// const addToFavorite = inject('addToFavorite')

// const onClickFavorite = () => {
//   const obj = {
//     ...props,
//     parentId: props.id
//   }
//   addToFavorite(obj)
// }
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :onClickAdd="onClickAdd"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :isFavorite="item.isFavorite"
    />
  </div>
</template>
