<template>
  <div class="flex min-h-screen">
    <aside class="w-64 bg-indigo-950 text-white p-5 flex flex-col">
      <h1 class="text-2xl font-bold mb-8">Admin Panel</h1>

      <nav class="flex flex-col gap-4">
        <NuxtLink to="/">Dashboard</NuxtLink>
        <NuxtLink to="/about">About</NuxtLink>
        <NuxtLink to="/education">Education</NuxtLink>
        <NuxtLink to="/projects">Projects</NuxtLink>
        <NuxtLink to="/skills">Skills</NuxtLink>
        <NuxtLink to="/socials">Socials</NuxtLink>
        <NuxtLink to="/contacts">Contacts</NuxtLink>
      </nav>

      <button
        @click="logout"
        class="mt-auto bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded"
      >
        Logout
      </button>
    </aside>

    <main class="flex-1 p-6 bg-gray-100">
      <slot />
    </main>
  </div>
</template>

<script setup>
const logout = () => {
  localStorage.removeItem('token')
  navigateTo('/login')
}
</script>