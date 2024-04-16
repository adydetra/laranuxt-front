<script setup lang="ts">
useHead({
  meta: [
    {
      name: 'msapplication-TileColor',
      content: '#da532c',
    },
    {
      name: 'theme-color',
      content: '#ffffff',
    },
  ],
});

useSeoMeta({
  description: 'Laravel x Nuxt',
  twitterCard: 'summary_large_image',
});
</script>

<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
