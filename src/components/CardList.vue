<script setup>
import Card from './Card.vue'
</script>
<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="5000"
    /><Card /><Card /><Card /><Card /><Card /><Card />
  </div>
</template>
