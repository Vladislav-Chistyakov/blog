<script setup lang="ts">
  import {useAsyncData} from "#app";

  const msg = 'Hello!'
  const url = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'

  const { data } = await useAsyncData('data', () => $fetch(url))


  let arr = ref([])
  function log() {
    console.log(msg)
    console.warn('arr: ', arr)
  }


</script>

<template>
  <div>
    <ul>
      <li>
        <button @click="log">{{ msg }}</button>
      </li>
      <li v-if="Array.isArray(arr) && arr.length" v-for="(item, index) in arr" :key="index">
        <NuxtLink :to="`/card/${item}`">Blog: {{item}}</NuxtLink>
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
