<template>
  <div>
    <h1 class="text-3xl font-bold mb-6">Dashboard</h1>

    <div class="grid grid-cols-3 gap-5">
      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Abouts</h2>
        <p class="text-3xl">{{ abouts }}</p>
      </div>

      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Education</h2>
        <p class="text-3xl">{{ education }}</p>
      </div>

      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Projects</h2>
        <p class="text-3xl">{{ projects }}</p>
      </div>

      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Skills</h2>
        <p class="text-3xl">{{ skills }}</p>
      </div>

      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Socials</h2>
        <p class="text-3xl">{{ socials }}</p>
      </div>

      <div class="bg-white p-5 rounded shadow">
        <h2 class="text-xl font-semibold">Contacts</h2>
        <p class="text-3xl">{{ contacts }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>

definePageMeta({
  middleware: 'auth'
})

const abouts = ref(0)
const education = ref(0)
const projects = ref(0)
const skills = ref(0)
const socials = ref(0)
const contacts = ref(0)

const getStatistics = async () => {
  const aboutsData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/abouts')
  const educationData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/education')
  const projectsData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/projects')
  const skillsData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/skills')
  const socialsData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/socials')
  const contactsData = await $fetch('https://portfolio-backend-vn5k.onrender.com/api/contact')

  abouts.value = aboutsData.length
  education.value = educationData.length
  projects.value = projectsData.length
  skills.value = skillsData.length
  socials.value = socialsData.length
  contacts.value = contactsData.length
}

onMounted(() => {
  getStatistics()
})
</script>