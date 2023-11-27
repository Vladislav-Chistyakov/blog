<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps({
  activePage: {
    type: Number,
    default: 1
  },
  cardNumber: {
    type: Number,
    default: 1
  },
  dataList: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['newPage'])

// Переключение страниц
const funcNewPage = function (newPage:number) {
  emit('newPage', newPage)
}

// Показ кнопок следующей страницы и предыдущей страницы
const activeNextPage = computed(() => props.activePage < numberSwitchings.value ? true : false)
const activePreviousPage = computed(() => props.activePage !== 1 ? true : false)

// Вывод количества переключений для пагинации
const numberSwitchings = computed(() => Math.ceil(props.dataList.length / props.cardNumber))

// Массив пагинации
const paginationArray = computed(() => {
  if (numberSwitchings.value) {
    const array:number[] = []
    for (let i:number = 1; i <= numberSwitchings.value; i++) {
      array.push(i)
    }
    return array
  } else {
    return []
  }
})
</script>

<template>
  <div class="tw-flex tw-flex-row tw-gap-[8px]">
    <button v-if="activePreviousPage"
            class="tw-transition-all tw-pt-[8px] tw-pr-[8px] tw-pb-[10px] tw-pl-[10px] tw-border tw-border-[#E8E8E8] tw-rounded-[12px] tw-h-fit tw-rotate-180 hover:tw-bg-[#E8E8E8]"
            @click="funcNewPage(props.activePage - 1)"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M9.5 7.5L14.5 12.5L9.5 17.5" stroke="#494949" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
    <ul class="tw-flex tw-flex-row tw-gap-[8px]">
      <li v-for="(item, index) in paginationArray"
          :key="index"
      >
        <button class="tw-transition-all tw-px-[16px] tw-py-[10px] tw-border tw-border-[#F3F3F3] tw-bg-[#F3F3F3] tw-rounded-[12px] hover:tw-border-[#E8E8E8] hover:tw-bg-[#E8E8E8] active:tw-border-[#101010] active:tw-bg-[#101010] active:tw-text-white"
                :class="{ 'tw-border-[#101010] tw-bg-[#101010] tw-text-white' : props.activePage === item }"
                @click="funcNewPage(item)"
        >
          {{ item }}
        </button>
      </li>
    </ul>
    <button v-if="activeNextPage"
            class="tw-transition-all tw-pt-[8px] tw-pr-[8px] tw-pb-[10px] tw-pl-[10px] tw-border tw-border-[#E8E8E8] tw-rounded-[12px] tw-h-fit hover:tw-bg-[#E8E8E8]"
            @click="funcNewPage(props.activePage + 1)"
    >
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M9.5 7.5L14.5 12.5L9.5 17.5" stroke="#494949" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </button>
  </div>
</template>

<style scoped>

</style>
