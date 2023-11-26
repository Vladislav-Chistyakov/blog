<script setup lang="ts">
import {useAsyncData} from "#app";
import {computed} from "vue";

const API = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'
const dataList = ref()
const {data, pending, error} = await useAsyncData('list', () => $fetch(API))
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

const oldPage = function () {
  if (activePage.value > 1) {
    activePage.value--
  } else {
    activePage.value = 1
  }
}

const activeData = computed(() => {
  if (!(activePage.value - 1)) {
    return {
      pages: dataList.value.slice(activePage.value - 1, cardNumber.value),
      numberCard: cardNumber.value,
      indexPage: activePage.value - 1
    }
  } else {
    return {
      pages: dataList.value.slice((activePage.value - 1) * cardNumber.value , cardNumber.value * activePage.value),
      numberCard: cardNumber.value * activePage.value,
      indexPage: (activePage.value - 1) * cardNumber.value
    }
  }
})

const pageCount = ref(0)

const numberPage = computed(() => Math.ceil(dataList.value.length / cardNumber.value))

const pag = computed(() => {
  if (numberPage.value) {
    const arrayPageNumber:number[] = []
    console.log(numberPage.value)
    for (let i:number = 1; i <= numberPage.value; i++) {
      arrayPageNumber.push(i)
    }
    return arrayPageNumber
  } else {
    return []
  }
})
pageCount.value = numberPage.value

</script>

<template>
  <div>
    <div v-if="pending">
      Загрузка
    </div>
    <div v-else>
      <div class="tw-flex tw-flex-row">
        <button class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
                @click="oldPage"
        >
          ' &lt; '
        </button>
        <ul class="tw-flex tw-flex-row">
          <li v-for="(item, index) in pag"
              :key="index"
          >
            <button
                class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
                :class="{ 'tw-bg-blog-red-main' : activePage === item }"
                @click="activePage = item"
            >
              {{ item }}
            </button>
          </li>
        </ul>
        <button
            class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
            @click="nextPage">
          ' > '
        </button>
      </div>
      <strong class="tw-text-amber-950 tw-p-[10px] tw-m-5 tw-block">
        {{ activePage }}
      </strong>
      <ul v-if="Array.isArray(activeData.pages)">
        <li v-for="(item, index) in activeData.pages"
            :key="index"
            class="tw-p-[30px] tw-border tw-border-blog-red-main tw-mb-[10px]">
          <div class="tw-p-4">
            indexPage: {{ activeData.indexPage }} || numberCard: {{ activeData.numberCard }}
          </div>
          <div class="tw-p-4">
            {{ item.id }}
          </div>
        </li>
      </ul>
      <div>
        Go to Page
      </div>
    </div>
  </div>
</template>
