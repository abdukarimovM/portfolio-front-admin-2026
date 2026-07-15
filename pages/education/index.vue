<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Education</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">

        <input v-model="name" placeholder="Name" class="border p-2" />

        <input v-model="link" placeholder="Link" class="border p-2" />

        <!-- IMAGE -->
        <div class="col-span-2">

          <input
            type="file"
            @change="uploadImage"
            class="border p-2 w-full"
            accept="image/*"
          />

          <img
            v-if="icon"
            :src="`http://localhost:3001${icon}`"
            class="w-32 h-32 object-contain mt-4 border rounded"
          />

        </div>

        <input v-model="startYear" placeholder="Start Year" class="border p-2" />
        <input v-model="endYear" placeholder="End Year" class="border p-2" />

        <input
          v-model="direction"
          placeholder="Direction"
          class="border p-2 col-span-2"
        />

      </div>

      <button
        @click="saveEducation"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        {{ editId ? 'Update Education' : 'Add Education' }}
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
          <th class="border p-2">Direction</th>
          <th class="border p-2">Years</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in educations" :key="item.id">

          <td class="border p-2">{{ item.name }}</td>
          <td class="border p-2">{{ item.direction }}</td>

          <td class="border p-2">
            {{ item.startYear }} - {{ item.endYear }}
          </td>

          <td class="border p-2">

            <button
              @click="editEducation(item)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-2"
            >
              Edit
            </button>

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
definePageMeta({ middleware: 'auth' })

const educations = ref([])
const editId = ref(null)

const name = ref('')
const link = ref('')
const icon = ref('')

const startYear = ref('')
const endYear = ref('')
const direction = ref('')

const getEducations = async () => {
  educations.value = await $fetch('http://localhost:3001/api/education')
}

const clearForm = () => {
  editId.value = null
  name.value = ''
  link.value = ''
  icon.value = ''
  startYear.value = ''
  endYear.value = ''
  direction.value = ''
}

const editEducation = (item) => {
  editId.value = item.id
  name.value = item.name
  link.value = item.link
  icon.value = item.icon
  startYear.value = item.startYear
  endYear.value = item.endYear
  direction.value = item.direction
}

const cancelEdit = () => clearForm()

const saveEducation = async () => {

  const payload = {
    name: name.value,
    link: link.value,
    icon: icon.value,
    startYear: startYear.value,
    endYear: endYear.value,
    direction: direction.value,
  }

  if (editId.value) {
    await $fetch(`http://localhost:3001/api/education/${editId.value}`, {
      method: 'PATCH',
      body: payload,
    })
  } else {
    await $fetch('http://localhost:3001/api/education', {
      method: 'POST',
      body: payload,
    })
  }

  clearForm()
  getEducations()
}

const uploadImage = async (event) => {
  const file = event.target.files[0]
  if (!file) return

  const formData = new FormData()
  formData.append('file', file)

  const res = await $fetch('http://localhost:3001/api/upload', {
    method: 'POST',
    body: formData,
  })

  icon.value = res.url
}

onMounted(getEducations)
</script>