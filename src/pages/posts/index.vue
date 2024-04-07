<script setup lang="ts">
definePageMeta({
  layout: 'post',
});

useHead({
  title: 'Data Posts',
});

const config = useRuntimeConfig();

const { data: posts }: any = await useAsyncData('posts', () => $fetch(`${config.public.apiBase}/api/posts`));

async function deletePost(id: number) {
  await $fetch(`${config.public.apiBase}/api/posts/${id}`, {
    method: 'DELETE',
  });
  refreshNuxtData('posts');
}
</script>

<template>
  <div class="container mt-5 mb-5">
    <UButton to="/posts/create" color="sky" variant="soft" class="btn btn-md btn-success rounded shadow border-0 mb-3">
      Add Post
    </UButton>
    <table class="border-collapse table-fixed w-full">
      <thead>
        <tr>
          <th>Image</th>
          <th>Title</th>
          <th>Content</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
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
</template>

<style scoped>
td {
  padding: 1.5rem 0;
}
</style>
