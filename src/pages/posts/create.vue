<script setup lang="ts">
useHead({
  title: 'Create',
});

const config = useRuntimeConfig();
const router = useRouter();

// state
const image = ref('');
const title = ref('');
const content = ref('');
const errors: any = ref({});

// method for handle file changes
function handleFileChange(e: any) {
  // assign file to state
  image.value = e.target.files[0];
}

async function storePost() {
  // init formData
  const formData = new FormData();

  // assign state value to formData
  formData.append('image', image.value);
  formData.append('title', title.value);
  formData.append('content', content.value);

  // store data with API
  await $fetch(`${config.public.apiBase}/api/posts`, {
    method: 'POST',
    body: formData,
  })
    .then(() => {
      router.push({ path: '/posts' });
    })
    .catch((error) => {
      // assign response error data to state "errors"
      errors.value = error.data;
    });
}
</script>

<template>
  <Back />
  <form @submit.prevent="storePost()">
    <div class="space-y-3">
      <label>Image</label>
      <input type="file" class="form-control" @change="handleFileChange($event)">
      <div v-if="errors.image" class="alert alert-danger mt-2">
        <span>{{ errors.image[0] }}</span>
      </div>
    </div>
    <div class="space-y-3">
      <label>Title</label>
      <input v-model="title" type="text" class="form-control" placeholder="Title Post">
      <div v-if="errors.title" class="alert alert-danger mt-2">
        <span>{{ errors.title[0] }}</span>
      </div>
    </div>
    <div class="space-y-3">
      <label>Content</label>
      <UTextarea v-model="content" class="form-control" rows="5" placeholder="Content Post" />
      <div v-if="errors.content" class="alert alert-danger mt-2">
        <span>{{ errors.content[0] }}</span>
      </div>
    </div>
    <UButton type="submit" color="sky" variant="soft" class="mt-3">
      Save
    </UButton>
  </form>
</template>
