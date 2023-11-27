<script setup lang="ts">
import { useAsyncData } from '#app';
import { computed } from 'vue';

const API = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'
const dataList = ref()

const { data, pending, error } = await useAsyncData('list', () => $fetch(API))

if (error) console.error('Error card page: ', error)
dataList.value = data.value

const activePage = ref(1)
const cardNumber = ref(8)

// Вывод необходимого количества карточек на страницу
const cardOutput = computed(() => {
  if (!(activePage.value - 1)) {
    return dataList.value.slice(activePage.value - 1, cardNumber.value)
  } else {
    return dataList.value.slice((activePage.value - 1) * cardNumber.value , cardNumber.value * activePage.value)
  }
})

</script>

<template>
  <div class="tw-pt-[120px] tw-pb-[140px]">
    <h1 class="tw-text-[84px] tw-mb-[60px]">
      Articles
    </h1>
    <div v-if="pending" class="tw-text-[34px] tw-w-fit tw-mx-auto">
      Loading...
    </div>
    <div v-else-if="error"
         class="tw-text-blog-red-main tw-text-[34px] tw-w-fit tw-mx-aut tw-p-20"
    >
      Error: {{ error }}
    </div>
    <div v-else>
      <ul v-if="Array.isArray(cardOutput)"
          class="tw-grid tw-grid-cols-4 tw-gap-[32px] tw-mb-[50px]"
      >
        <li v-for="(item, index) in cardOutput"
            :key="index"
            class="tw-group tw-transition-all hover:-tw-translate-y-6">
          <div v-if="item.image && item.title" class="tw-mb-[24px]">
            <img :src="item.image" :alt="item.title" class="tw-bg-blog-border-solid tw-min-h-[280px]">
          </div>
          <div v-if="item.preview" class="tw-text-[20xp]">
            {{ item.preview }}
          </div>
          <NuxtLink :to="`/card/${item.id}`"
                    class="tw-transition-all tw-none group-hover:tw-block tw-opacity-0 group-hover:tw-opacity-100 tw-text-blog-text-purple hover:tw-underline">
            Read more
          </NuxtLink>
        </li>
      </ul>
      <PaginationBlock :data-list="dataList"
                       :active-page="activePage"
                       :card-number="cardNumber"
                       @new-page="activePage = $event"
      />
    </div>
  </div>
</template>
