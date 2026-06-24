<template>
  <div>
    <h1 class="text-3xl font-bold mb-5">Skills</h1>

    <div class="bg-white p-5 rounded shadow mb-5">
      <div class="grid grid-cols-2 gap-3">
        <input
          v-model="title"
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
        @click="addSkill"
        class="mt-4 bg-indigo-700 text-white px-4 py-2 rounded"
      >
        Add Skill
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
        <tr v-for="item in skills" :key="item.id">
          <td class="border p-2">{{ item.title }}</td>

          <td class="border p-2">
            {{ item.icon }}
          </td>

          <td class="border p-2">
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

const title = ref('')
const icon = ref('')

const getSkills = async () => {
  skills.value = await $fetch('http://localhost:3001/api/skills')
}

const addSkill = async () => {
  await $fetch('http://localhost:3001/api/skills', {
    method: 'POST',
    body: {
      title: title.value,
      icon: icon.value,
    },
  })

  title.value = ''
  icon.value = ''

  await getSkills()
}

const deleteSkill = async (id) => {
  await $fetch(`http://localhost:3001/api/skills/${id}`, {
    method: 'DELETE',
  })

  await getSkills()
}

onMounted(() => {
  getSkills()
})
</script>