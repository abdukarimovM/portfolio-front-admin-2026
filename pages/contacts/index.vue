<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Contacts</h1>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="border p-2">Name</th>
          <th class="border p-2">Email</th>
          <th class="border p-2">Title</th>
          <th class="border p-2">Message</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in contacts" :key="item.id">
          <td class="border p-2">{{ item.name }}</td>
          <td class="border p-2">{{ item.email }}</td>
          <td class="border p-2">{{ item.title }}</td>
          <td class="border p-2">{{ item.message }}</td>

          <td class="border p-2">
            <button
              @click="deleteContact(item.id)"
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

const contacts = ref([])

const getContacts = async () => {
  contacts.value = await $fetch(
    'https://portfolio-backend-vk4j.onrender.com/api/contact'
  )
}

const deleteContact = async (id) => {
  await $fetch(
    `https://portfolio-backend-vk4j.onrender.com/api/contact/${id}`,
    {
      method: 'DELETE',
    }
  )

  await getContacts()
}

onMounted(() => {
  getContacts()
})
</script>