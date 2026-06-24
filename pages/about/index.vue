<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Abouts</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <input
        v-model="description"
        type="text"
        placeholder="Description"
        class="border p-2 mr-2"
      />

      <input
        v-model="image"
        type="text"
        placeholder="Image URL"
        class="border p-2 mr-2"
      />

      <button
        @click="addAbout"
        class="bg-indigo-700 text-white px-4 py-2 rounded"
      >
        Add
      </button>
    </div>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="border p-2">ID</th>
          <th class="border p-2">Description</th>
          <th class="border p-2">Image</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in abouts" :key="item.id">
          <td class="border p-2">{{ item.id }}</td>
          <td class="border p-2">{{ item.description }}</td>
          <td class="border p-2">{{ item.image }}</td>

          <td class="border p-2">
            <button
              @click="deleteAbout(item.id)"
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

const abouts = ref([])
const description = ref('')
const image = ref('')

const getAbouts = async () => {
  abouts.value = await $fetch(
    'http://localhost:3001/api/abouts'
  )
}

const addAbout = async () => {
  await $fetch(
    'http://localhost:3001/api/abouts',
    {
      method: 'POST',
      body: {
        description: description.value,
        image: image.value,
      },
    }
  )

  description.value = ''
  image.value = ''

  getAbouts()
}

const deleteAbout = async (id) => {
  await $fetch(
    `http://localhost:3001/api/abouts/${id}`,
    {
      method: 'DELETE',
    }
  )

  getAbouts()
}

onMounted(() => {
  getAbouts()
})
</script>
