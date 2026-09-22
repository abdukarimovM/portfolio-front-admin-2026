<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Projects</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">
        <input
          v-model="title"
          placeholder="Title"
          class="border p-2"
        />

        <div class="col-span-2">

  <input
    type="file"
    @change="uploadImage"
    accept="image/*"
    class="border p-2 w-full"
  />

  <img
  v-if="image"
  :src="`https://portfolio-backend-vk4j.onrender.com${image}`"
  class="w-44 h-28 mt-4 object-cover rounded border"
/>

</div>

        <textarea
          v-model="description"
          placeholder="Description"
          rows="4"
          class="border p-2 col-span-2"
        ></textarea>

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
  @click="saveProject"
  :disabled="uploading"
  class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded disabled:opacity-50"
>
  {{ uploading ? 'Uploading...' : (editId ? 'Update Project' : 'Add Project') }}
</button>

      <button
        v-if="editId"
        @click="cancelEdit"
        class="mt-4 ml-3 bg-gray-500 text-white px-4 py-2 rounded"
      >
        Cancel
      </button>
    </div>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
           <th class="border p-2">Image</th>
    <th class="border p-2">Title</th>
    <th class="border p-2">Direction</th>
    <th class="border p-2">Preview</th>
    <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="item in projects"
          :key="item.id"
        >

          <td class="border p-2">
  <img
    :src="`https://portfolio-backend-vk4j.onrender.com${item.image}`"
    class="w-24 h-16 object-cover rounded"
  />
</td>

          <td class="border p-2">
            {{ item.title }}
          </td>

          <td class="border p-2">
            {{ item.direction }}
          </td>

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
              @click="editProject(item)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-2"
            >
              Edit
            </button>

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

const editId = ref(null)

const title = ref('')
const image = ref('')

const uploading = ref(false)

const description = ref('')
const preview = ref('')
const direction = ref('')

const getProjects = async () => {
  try {
    projects.value = await $fetch(
      'https://portfolio-backend-vk4j.onrender.com/api/projects'
    )
  } catch (error) {
    console.error(error)
  }
}

const clearForm = () => {
  editId.value = null

  title.value = ''
  image.value = ''
  description.value = ''
  preview.value = ''
  direction.value = ''
}

const editProject = (item) => {
  editId.value = item.id

  title.value = item.title
  image.value = item.image
  description.value = item.description
  preview.value = item.preview
  direction.value = item.direction
}

const cancelEdit = () => {
  clearForm()
}

const uploadImage = async (event) => {

  const file = event.target.files[0]

  if (!file) return

  uploading.value = true

  const formData = new FormData()

  formData.append('file', file)

  try {

    const res = await $fetch(
      'https://portfolio-backend-vk4j.onrender.com/api/upload',
      {
        method: 'POST',
        body: formData,
      }
    )

    image.value = res.url

  } catch (error) {

    console.error(error)

  } finally {

    uploading.value = false

  }
}

const saveProject = async () => {

  if (editId.value) {

    await $fetch(
      `https://portfolio-backend-vk4j.onrender.com/api/projects/${editId.value}`,
      {
        method: 'PUT',
        body: {
          title: title.value,
          image: image.value,
          description: description.value,
          preview: preview.value,
          direction: direction.value,
        },
      }
    )

  } else {

    await $fetch(
      'https://portfolio-backend-vk4j.onrender.com/api/projects',
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

  }

  clearForm()

  getProjects()
}

const deleteProject = async (id) => {
  await $fetch(
    `https://portfolio-backend-vk4j.onrender.com/api/projects/${id}`,
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