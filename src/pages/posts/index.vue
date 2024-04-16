<script setup lang="ts">
definePageMeta({
  layout: 'post',
});

useHead({
  title: 'Data Posts',
});

const config = useRuntimeConfig();

const { pending, data: posts }: any = await useLazyAsyncData('posts', () => $fetch(`${config.public.apiBase}/api/posts`));

async function deletePost(id: number) {
  await $fetch(`${config.public.apiBase}/api/posts/${id}`, {
    method: 'DELETE',
  });
  refreshNuxtData('posts');
}
</script>

<template>
  <div class="container mt-5 mb-5">
    <div class="flex justify-between items-center">
      <AppBack />
      <div class="space-x-6 flex items-center">
        <UButton to="/posts/create" color="sky" variant="soft">
          Add Post
        </UButton>
        <AppColorMode />
      </div>
    </div>
    <div v-if="pending" class="mt-12 text-center">
      Loading ...
    </div>
    <div v-else>
      <table class="table-fixed w-full mt-12">
        <thead>
          <tr>
            <th>Image</th>
            <th>Title</th>
            <th>Content</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 dark:divide-gray-800">
          <tr v-for="(post, index) in posts.data.data" :key="index">
            <td>
              <NuxtImg :src="post.image" class="size-20 rounded-xl mx-auto" />
            </td>
            <td class="text-center">
              {{ post.title }}
            </td>
            <td class="text-center">
              {{ post.content }}
            </td>
            <td class="space-x-4 text-center">
              <UButton icon="i-heroicons-pencil-square-16-solid" size="sm" color="yellow" variant="soft" :to="`/posts/edit/${post.id}`" />
              <UButton icon="i-heroicons-trash" size="sm" color="red" variant="soft" @click="deletePost(post.id)" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
td {
  padding: 1.5rem 0;
}
</style>
