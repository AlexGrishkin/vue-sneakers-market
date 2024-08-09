<script setup>
import { inject, ref } from 'vue'

const { onChangeSelect } = inject('select')

const dropDownOpen = ref(false)

const handlerToogle = () => {
  dropDownOpen.value = !dropDownOpen.value
}
</script>

<template>
  <div @click="handlerToogle" class="relative">
    <div
      class="py-2 px-6 outline outline-1 outline-gray-200 rounded-md flex gap-4 justify-between items-center cursor-pointer"
    >
      <p>Отфильтровать</p>
      <img :class="`transition ${dropDownOpen ? 'rotate-180' : null}`" src="/ArrowFilter.svg" />
    </div>

    <ul
      v-if="dropDownOpen"
      class="z-50 py-2 absolute w-full outline outline-1 outline-gray-200 rounded-md mt-px flex flex-col gap-2 bg-white"
    >
      <li
        @click="onChangeSelect('name')"
        class="pl-3 pr-2 cursor-pointer hover:bg-slate-200"
        value="name"
      >
        По названию
      </li>
      <li
        @click="onChangeSelect('price')"
        class="pl-3 pr-2 cursor-pointer hover:bg-slate-200"
        value="price"
      >
        По цене (дешевые)
      </li>
      <li
        @click="onChangeSelect('-price')"
        class="pl-3 pr-2 cursor-pointer hover:bg-slate-200"
        value="-price"
      >
        По цене (дорогие)
      </li>
    </ul>
  </div>
</template>
