<script setup lang="ts">
import {useAsyncData} from "#app";
import {computed} from "vue";

const API = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'
const dataList = ref()
const {data, pending, error, refresh} = await useAsyncData('list', () => $fetch(API))
dataList.value = data.value

const activePage = ref(1)
const cardNumber = ref(8)

const nextPage = function () {
  if (activePage.value < numberPage.value) {
     activePage.value++
  } else {
    activePage.value = 1
  }
}

const activeData = computed(() => {
  return dataList.value.slice(activePage.value * cardNumber.value - 1, activePage.value * cardNumber.value * 2)
})

const pageCount = ref(0)

const numberPage = computed(() => Math.floor(dataList.value.length / 8))
pageCount.value = numberPage.value
const pages = ref([])

</script>

<template>
  <div>
    <button class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn" @click="nextPage">` > `</button>
    <strong class="tw-text-amber-950 tw-p-[10px] tw-m-5 tw-block">{{ activePage }}</strong>
    <ul class="tw-flex tw-flex-row tw-py-[50px]">
      <li>
        <button class="tw-mx-[30px] tw-p-[10px] tw-bg-blog-red-main tw-text-white">
          1
        </button>
      </li>
      <li>
        <button class="tw-mx-[30px] tw-p-[10px] tw-bg-blog-red-main tw-text-white">
          2
        </button>
      </li>

    </ul>
    <ul v-if="Array.isArray(activeData)">
      <li v-for="(item, index) in activeData" :key="index" class="tw-p-[30px]">
        <div>{{ item }}</div>
      </li>
    </ul>
    <div>Go to Page</div>

  </div>
</template>

<style scoped>

</style>
