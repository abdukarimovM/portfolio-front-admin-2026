<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Skills</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">
        <input
          v-model="name"
          placeholder="Skill Name"
          class="border p-2"
        />

        <input
          v-model="icon"
          placeholder="Icon URL"
          class="border p-2"
        />
      </div>

      <button
        @click="saveSkill"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        {{ editId ? 'Update Skill' : 'Add Skill' }}
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
          <th class="border p-2">Icon</th>
          <th class="border p-2">Action</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="item in skills"
          :key="item.id"
        >
          <td class="border p-2">
            {{ item.name }}
          </td>

          <td class="border p-2">
            {{ item.icon }}
          </td>

          <td class="border p-2">

            <button
              @click="editSkill(item)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-2"
            >
              Edit
            </button>

            <button
              @click="deleteSkill(item.id)"
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

const skills = ref([])

const editId = ref(null)

const name = ref('')
const icon = ref('')

const getSkills = async () => {
  skills.value = await $fetch(
    'https://portfolio-backend-vn5k.onrender.com/api/skills'
  )
}

const clearForm = () => {
  editId.value = null

  name.value = ''
  icon.value = ''
}

const editSkill = (item) => {
  editId.value = item.id

  name.value = item.name
  icon.value = item.icon
}

const cancelEdit = () => {
  clearForm()
}

const saveSkill = async () => {

  if (editId.value) {

    await $fetch(
      `https://portfolio-backend-vn5k.onrender.com/api/skills/${editId.value}`,
      {
        method: 'PUT',
        body: {
           name: name.value,
  icon: icon.value,
        },
      }
    )

  } else {

    await $fetch(
      'https://portfolio-backend-vn5k.onrender.com/api/skills',
      {
        method: 'POST',
        body: {
           name: name.value,
  icon: icon.value,
        },
      }
    )

  }

  clearForm()

  getSkills()
}

const deleteSkill = async (id) => {
  await $fetch(
    `https://portfolio-backend-vn5k.onrender.com/api/skills/${id}`,
    {
      method: 'DELETE',
    }
  )

  getSkills()
}

onMounted(() => {
  getSkills()
})
</script>