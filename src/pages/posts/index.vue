<script setup lang="ts">
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
    <div class="card border-0 rounded shadow">
      <div class="card-body">
        <table class="table table-bordered w-full">
          <thead class="text-white">
            <tr>
              <th scope="col">
                Image
              </th>
              <th scope="col">
                Title
              </th>
              <th scope="col">
                Content
              </th>
              <th scope="col" style="width:15%">
                Actions
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(post, index) in posts.data.data" :key="index">
              <td>
                <NuxtImg :src="post.image" class="size-80 rounded-xl mx-auto" />
              </td>
              <td class="text-center">
                {{ post.title }}
              </td>
              <td class="text-center">
                {{ post.content }}
              </td>
              <td class="space-x-4 flex">
                <UButton icon="i-heroicons-pencil-square-16-solid" size="sm" color="yellow" variant="soft" :to="`/posts/edit/${post.id}`" />
                <UButton icon="i-heroicons-trash" size="sm" color="red" variant="soft" @click="deletePost(post.id)" />
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
