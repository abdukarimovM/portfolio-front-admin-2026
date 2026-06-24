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
          class="border p-2"
        />
      </div>

      <button
        @click="addSocial"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        Add Social
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
        <tr v-for="item in socials" :key="item.id">
          <td class="border p-2">{{ item.name }}</td>

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

const name = ref('')
const link = ref('')
const icon = ref('')

const getSocials = async () => {
  socials.value = await $fetch(
    'http://localhost:3001/api/socials'
  )
}

const addSocial = async () => {
  await $fetch(
    'http://localhost:3001/api/socials',
    {
      method: 'POST',
      body: {
        name: name.value,
        link: link.value,
        icon: icon.value,
      },
    }
  )

  name.value = ''
  link.value = ''
  icon.value = ''

  await getSocials()
}

const deleteSocial = async (id) => {
  await $fetch(
    `http://localhost:3001/api/socials/${id}`,
    {
      method: 'DELETE',
    }
  )

  await getSocials()
}

onMounted(() => {
  getSocials()
})
</script>