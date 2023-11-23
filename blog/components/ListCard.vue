<script setup lang="ts">
  const msg = 'Hello!'
  function log() {
    console.log(msg)
  }
  const arr = [
    { name: 1}, { name: 2}
  ]
</script>

<template>
  <div>
    <ul>
      <li>
        <button @click="log">{{ msg }}</button>
      </li>
      <li v-for="(item, index) in arr">{{item.name}}</li>
    </ul>
  </div>
</template>

<style scoped>

</style>
