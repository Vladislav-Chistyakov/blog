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
const activePreviousPage = computed(() => props.activePage === 1 ? false : true)

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
  <div class="tw-flex tw-flex-row">
    <button v-if="activePreviousPage"
            class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
            @click="funcNewPage(props.activePage - 1)"
    >
      ' &lt; '
    </button>
    <ul class="tw-flex tw-flex-row">
      <li v-for="(item, index) in paginationArray"
          :key="index"
      >
        <button
            class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
            :class="{ 'tw-bg-blog-red-main' : props.activePage === item }"
            @click="funcNewPage(item)"
        >
          {{ item }}
        </button>
      </li>
    </ul>
    <button
        v-if="activeNextPage"
        class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
        @click="funcNewPage(props.activePage + 1)">
      ' > '
    </button>
  </div>
</template>

<style scoped>

</style>
