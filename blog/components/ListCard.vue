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
  <div class="tw-pt-[120px] tw-pb-[140px]">
    <h1 class="tw-text-[84px] tw-mb-[60px]">
      Articles
    </h1>
    <div v-if="pending">
      Загрузка
    </div>
    <div v-else>
      <ul v-if="Array.isArray(activeData.pages)"
          class="tw-grid tw-grid-cols-4 tw-gap-[32px] tw-mb-[50px]"
      >
        <li v-for="(item, index) in activeData.pages"
            :key="index"
            class="tw-group tw-transition-all hover:-tw-translate-y-6">
          <div v-if="item.image && item.title" class="tw-mb-[24px]">
            <img :src="item.image" :alt="item.title" class="tw-bg-blog-border-solid tw-min-h-[280px]">
          </div>
          <div v-if="item.preview" class="tw-text-[20xp]">
            {{ item.preview }}
          </div>
          <button class="tw-transition-all tw-none group-hover:tw-block tw-opacity-0 group-hover:tw-opacity-100 tw-text-blog-text-purple hover:tw-underline">
            Read more
          </button>
        </li>
      </ul>
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
    </div>
  </div>
</template>
