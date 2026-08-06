<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Socials</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">

        <input
          v-model="name"
          placeholder="Social Name"
          class="border p-2"
        />

        <input
          v-model="link"
          placeholder="Profile Link"
          class="border p-2"
        />

        <input
          v-model="icon"
          placeholder="Icon Class"
          class="border p-2 col-span-2"
        />

      </div>

      <button
        @click="saveSocial"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        {{ editId ? 'Update Social' : 'Add Social' }}
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
          <th class="border p-2">Name</th>
          <th class="border p-2">Link</th>
          <th class="border p-2">Icon</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="item in socials"
          :key="item.id"
        >
          <td class="border p-2">
            {{ item.name }}
          </td>

          <td class="border p-2">
            <a
              :href="item.link"
              target="_blank"
              class="text-blue-600"
            >
              Open
            </a>
          </td>

          <td class="border p-2">
            {{ item.icon }}
          </td>

          <td class="border p-2">

            <button
              @click="editSocial(item)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-2"
            >
              Edit
            </button>

            <button
              @click="deleteSocial(item.id)"
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

const socials = ref([])

const editId = ref(null)

const name = ref('')
const link = ref('')
const icon = ref('')

const getSocials = async () => {
  socials.value = await $fetch(
    'https://portfolio-backend-vn5k.onrender.com/api/socials'
  )
}

const clearForm = () => {
  editId.value = null

  name.value = ''
  link.value = ''
  icon.value = ''
}

const editSocial = (item) => {
  editId.value = item.id

  name.value = item.name
  link.value = item.link
  icon.value = item.icon
}

const cancelEdit = () => {
  clearForm()
}

const saveSocial = async () => {

  if (editId.value) {

    await $fetch(
      `https://portfolio-backend-vn5k.onrender.com/api/socials/${editId.value}`,
      {
        method: 'PUT', // Agar controller PATCH bo'lsa PATCH qilib o'zgartiring
        body: {
          name: name.value,
          link: link.value,
          icon: icon.value,
        },
      }
    )

  } else {

    await $fetch(
      'https://portfolio-backend-vn5k.onrender.com/api/socials',
      {
        method: 'POST',
        body: {
          name: name.value,
          link: link.value,
          icon: icon.value,
        },
      }
    )

  }

  clearForm()

  getSocials()
}

const deleteSocial = async (id) => {
  await $fetch(
    `https://portfolio-backend-vn5k.onrender.com/api/socials/${id}`,
    {
      method: 'DELETE',
    }
  )

  getSocials()
}

onMounted(() => {
  getSocials()
})
</script>