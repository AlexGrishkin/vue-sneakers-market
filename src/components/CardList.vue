<script setup>
import Card from './Card.vue'
const onClickAdd = () => {
  alert('Добавить')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="5000"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
