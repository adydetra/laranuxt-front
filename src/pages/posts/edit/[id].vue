<script setup lang="ts">
useHead({
  title: 'Edit',
});

const config = useRuntimeConfig();
const router = useRouter();
const route = useRoute();

const { data: post }: any = await useFetch(`${config.public.apiBase}/api/posts/${route.params.id}`);

// state
const image = ref('');
const title = ref(post.value.data.title);
const content = ref(post.value.data.content);
const errors: any = ref({});

// method for handle file changes
function handleFileChange(e: any) {
  // assign file to state
  image.value = e.target.files[0];
}

async function updatePost() {
  const formData = new FormData();

  // assign state value to formData
  formData.append('image', image.value);
  formData.append('title', title.value);
  formData.append('content', content.value);
  formData.append('_method', 'PATCH');

  // store data with API
  await $fetch(`${config.public.apiBase}/api/posts/${route.params.id}`, {
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
  <div class="container mt-5">
    <Back />
    <div class="card border-0 rounded shadow">
      <div class="card-body">
        <form @submit.prevent="updatePost()">
          <div class="mb-3">
            <label class="form-label fw-bold">Image</label>
            <input type="file" class="form-control" @change="handleFileChange($event)">
            <div v-if="errors.image" class="alert alert-danger mt-2">
              <span>{{ errors.image[0] }}</span>
            </div>
          </div>
          <div class="mb-3">
            <label class="form-label fw-bold">Title</label>
            <input v-model="title" type="text" class="form-control" placeholder="Title Post">
            <div v-if="errors.title" class="alert alert-danger mt-2">
              <span>{{ errors.title[0] }}</span>
            </div>
          </div>
          <div class="mb-3">
            <label class="form-label fw-bold">Content</label>
            <textarea v-model="content" class="form-control" rows="5" placeholder="Content Post" />
            <div v-if="errors.content" class="alert alert-danger mt-2">
              <span>{{ errors.content[0] }}</span>
            </div>
          </div>
          <UButton color="sky" variant="soft" type="submit" class="btn btn-md btn-primary rounded-sm shadow border-0">
            Update
          </UButton>
        </form>
      </div>
    </div>
  </div>
</template>
