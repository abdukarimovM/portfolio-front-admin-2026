<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Education</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">
        <input v-model="name" placeholder="Name" class="border p-2" />
        <input v-model="link" placeholder="Link" class="border p-2" />
        <input v-model="icon" placeholder="Icon URL" class="border p-2" />
        <input v-model="date" type="date" class="border p-2" />
        <input
          v-model="direction"
          placeholder="Direction"
          class="border p-2"
        />
      </div>

      <button
        @click="addEducation"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        Add Education
      </button>
    </div>

    <table class="w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="border p-2">Name</th>
          <th class="border p-2">Direction</th>
          <th class="border p-2">Date</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in educations" :key="item.id">
          <td class="border p-2">{{ item.name }}</td>
          <td class="border p-2">{{ item.direction }}</td>
          <td class="border p-2">
            {{ item.date?.slice(0, 10) }}
          </td>
          <td class="border p-2">
            <button
              @click="deleteEducation(item.id)"
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

const educations = ref([])

const name = ref('')
const link = ref('')
const icon = ref('')
const date = ref('')
const direction = ref('')

const getEducations = async () => {
  educations.value = await $fetch(
    'http://localhost:3001/api/education'
  )
}

const addEducation = async () => {
  await $fetch(
    'http://localhost:3001/api/education',
    {
      method: 'POST',
      body: {
        name: name.value,
        link: link.value,
        icon: icon.value,
        date: date.value,
        direction: direction.value,
      },
    }
  )

  name.value = ''
  link.value = ''
  icon.value = ''
  date.value = ''
  direction.value = ''

  getEducations()
}

const deleteEducation = async (id) => {
  await $fetch(
    `http://localhost:3001/api/education/${id}`,
    {
      method: 'DELETE',
    }
  )

  getEducations()
}

onMounted(() => {
  getEducations()
})
</script>