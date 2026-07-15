<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">About</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-4">

        <input
          v-model="fullName"
          type="text"
          placeholder="Full Name"
          class="border p-2"
        />

        <input
          v-model="profession"
          type="text"
          placeholder="Profession"
          class="border p-2"
        />

        <textarea
          v-model="description"
          placeholder="Description"
          rows="5"
          class="border p-2 col-span-2"
        ></textarea>

        <div class="col-span-2">
  <input
    type="file"
    @change="uploadImage"
    class="border p-2 w-full"
    accept="image/*"
  />

  <img
    v-if="image"
    :src="`http://localhost:3001${image}`"
    class="w-32 h-32 object-cover rounded mt-4 border"
  />
</div>

      </div>

      <button
        @click="saveAbout"
        class="mt-5 bg-indigo-700 text-white px-5 py-2 rounded"
      >
        {{ editId ? 'Update' : 'Save' }}
      </button>

      <button
        v-if="editId"
        @click="cancelEdit"
        class="mt-5 ml-3 bg-gray-500 text-white px-5 py-2 rounded"
      >
        Cancel
      </button>

    </div>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="border p-2">ID</th>
          <th class="border p-2">Full Name</th>
          <th class="border p-2">Profession</th>
          <th class="border p-2">Description</th>
          <th class="border p-2">Image</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="item in abouts"
          :key="item.id"
        >
          <td class="border p-2">{{ item.id }}</td>

          <td class="border p-2">
            {{ item.fullName }}
          </td>

          <td class="border p-2">
            {{ item.profession }}
          </td>

          <td class="border p-2">
            {{ item.description }}
          </td>

          <td class="border p-2">
  <img
    :src="`http://localhost:3001${item.image}`"
    class="w-20 h-20 object-cover rounded"
  />
</td>

          <td class="border p-2">

            <button
              @click="editAbout(item)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-2"
            >
              Edit
            </button>

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

const editId = ref(null)

const fullName = ref('')
const profession = ref('')
const description = ref('')
const image = ref('')

const uploading = ref(false)

const getAbouts = async () => {
  abouts.value = await $fetch(
    'http://localhost:3001/api/abouts'
  )
}

const clearForm = () => {
  editId.value = null

  fullName.value = ''
  profession.value = ''
  description.value = ''
  image.value = ''
}

const editAbout = (item) => {
  editId.value = item.id

  fullName.value = item.fullName
  profession.value = item.profession
  description.value = item.description
  image.value = item.image
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

  const res = await $fetch(
    'http://localhost:3001/api/upload',
    {
      method: 'POST',
      body: formData,
    }
  )

  image.value = res.url

  uploading.value = false
}

const saveAbout = async () => {

  if (editId.value) {

    await $fetch(
      `http://localhost:3001/api/abouts/${editId.value}`,
      {
        method: 'PATCH',
        body: {
          fullName: fullName.value,
          profession: profession.value,
          description: description.value,
          image: image.value,
        },
      }
    )

  } else {

    await $fetch(
      'http://localhost:3001/api/abouts',
      {
        method: 'POST',
        body: {
          fullName: fullName.value,
          profession: profession.value,
          description: description.value,
          image: image.value,
        },
      }
    )

  }

  clearForm()

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