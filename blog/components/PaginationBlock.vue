<script setup lang="ts">

// Переключение на следующую страницу
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

const nextPage  = function () {
  if (props.activePage < numberSwitchings.value) {
    emit('newPage', props.activePage + 1)
  } else {
    emit('newPage', 1)
  }
}

// Переключение на предыдущую страницу
const previousPage = function () {
  if (props.activePage > 1) {
    emit('newPage', props.activePage - 1)
  } else {
    emit('newPage', 1)
  }
}


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
    <button class="tw-p-5 tw-m-5 tw-bg-blog-blue-btn"
            @click="previousPage"
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
            @click="$emit('newPage', item)"
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
</template>

<style scoped>

</style>
