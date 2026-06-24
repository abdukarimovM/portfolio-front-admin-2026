<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Projects</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">
        <input v-model="title" placeholder="Title" class="border p-2" />
        <input v-model="image" placeholder="Image URL" class="border p-2" />
        <input
          v-model="description"
          placeholder="Description"
          class="border p-2"
        />
        <input
          v-model="preview"
          placeholder="Preview URL"
          class="border p-2"
        />
        <input
          v-model="direction"
          placeholder="Direction"
          class="border p-2"
        />
      </div>

      <button
        @click="addProject"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        Add Project
      </button>
    </div>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="border p-2">Title</th>
          <th class="border p-2">Direction</th>
          <th class="border p-2">Preview</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in projects" :key="item.id">
          <td class="border p-2">{{ item.title }}</td>
          <td class="border p-2">{{ item.direction }}</td>

          <td class="border p-2">
            <a
              :href="item.preview"
              target="_blank"
              class="text-blue-600"
            >
              Open
            </a>
          </td>

          <td class="border p-2">
            <button
              @click="deleteProject(item.id)"
              class="bg-red-600 text-white px-3 py-1 rounded"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
definePageMeta({
  middleware: 'auth'
})

const projects = ref([])

const title = ref('')
const image = ref('')
const description = ref('')
const preview = ref('')
const direction = ref('')

const getProjects = async () => {
  projects.value = await $fetch(
    'http://localhost:3001/api/projects'
  )
}

const addProject = async () => {
  await $fetch(
    'http://localhost:3001/api/projects',
    {
      method: 'POST',
      body: {
        title: title.value,
        image: image.value,
        description: description.value,
        preview: preview.value,
        direction: direction.value,
      },
    }
  )

  title.value = ''
  image.value = ''
  description.value = ''
  preview.value = ''
  direction.value = ''

  getProjects()
}

const deleteProject = async (id) => {
  await $fetch(
    `http://localhost:3001/api/projects/${id}`,
    {
      method: 'DELETE',
    }
  )

  getProjects()
}

onMounted(() => {
  getProjects()
})
</script>