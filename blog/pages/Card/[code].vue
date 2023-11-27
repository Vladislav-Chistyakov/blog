<script setup lang="ts">
  import { useAsyncData } from '#app';

  const route = useRoute()
  const API = 'https://6082e3545dbd2c001757abf5.mockapi.io/qtim-test-work/posts/'
  const info = ref()

  const {data, pending, error} = await useAsyncData('list', () => $fetch(`${API}/${route.params.code}`))

  if (error) console.error('Error card page: ', error)
  info.value = data.value
</script>

<template>
  <div class="tw-pt-[120px] tw-px-[112px] tw-pb-[80px]">
    <div v-if="pending"
         class="tw-text-[34px] tw-w-fit tw-mx-auto tw-p-20"
    >
      Loading...
    </div>
    <div v-else-if="error"
         class="tw-text-blog-red-main tw-text-[34px] tw-w-fit tw-mx-aut tw-p-20"
    >
      Error: {{ error }}
    </div>
    <div v-else>
      <h1 v-if="info.title"
          class="tw-text-[84px] tw-mb-[73px]"
      >
        {{ info.title }}
      </h1>
      <div v-if="info.image && info.title"
           class="tw-mb-[80px]"
      >
        <img :src="info.image"
             :alt="info.title"
             class="tw-w-full"
        >
      </div>
      <div v-if="info.description">
        <span class="tw-block tw-mb-[32px] tw-text-[16px]">
          About
        </span>
        <p class="tw-max-w-[55%] tw-text-[36px]">
          {{ info.description }}
        </p>
      </div>
    </div>
  </div>
</template>
