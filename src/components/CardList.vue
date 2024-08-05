<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const onClickAdd = () => {
  alert('Добавить')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
