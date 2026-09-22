<template>
    <body>
      <div class="container mx-auto">
        <section class="">
          <div
            class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0"
          >
            <div
              class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0"
            >
              <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                <h1 class="text-4xl font-bold leading-tight text-[#1e1b4b] ml-[135px]">
                  Login
                </h1>
                <form class="space-y-4 md:space-y-6" @submit.prevent="login">
                  <div>
                    <label
                      for="email"
                      class="block mb-2 text-xl font-medium text-[#1e1b4b] dark:text-white"
                      >Email</label
                    >
                    <input
                      v-model="username"
                      type="email"
                      name="email"
                      id="email"
                      class="bg-gray-50 border border-indigo-500 text-gray-900 sm:text-sm rounded-lg focus:ring-indigo-600 focus:border-indigo-600 block w-full p-2.5"
                      placeholder="john@gmail.com"
                      required=""
                    />
                  </div>
                  <div>
                    <label
                      for="password"
                      class="block mb-2 text-sm font-medium text-indigo-950 dark:text-white"
                      >Password</label
                    >
                    <input
                      v-model="password"
                      type="password"
                      name="password"
                      id="password"
                      placeholder="••••••••"
                      class="bg-gray-50 border border-indigo-500 text-gray-900 sm:text-sm rounded-lg focus:ring-indigo-600 focus:border-indigo-600 block w-full p-2.5"
                      required=""
                    />
                  </div>
  
                  <button
                    type="submit"
                  class="mt-[50px] w-full bg-indigo-800 text-xl text-white hover:bg-indigo-950 focus:ring-4 focus:outline-none rounded-lg px-5 py-2.5 text-center"
                  >
                  Login
                  </button>
                </form>
              </div>
            </div>
          </div>
        </section>
      </div>
    </body>
  </template>
  
  <script setup>
  const username = ref('')
    const password = ref('')

  const login = async () => {
  try {
    const res = await $fetch(
      'https://portfolio-backend-vk4j.onrender.com/api/admin/auth/login',
      {
        method: 'POST',
        body: {
          username: username.value,
          password: password.value,
        },
      }
    )

    localStorage.setItem('token', res.data.token)

    await navigateTo('/')
  } catch (err) {
    console.log(err)
    alert('Login yoki parol xato')
  }
  }
  </script>
  
  <style lang="css" scoped>
  body {
    background: #050524;
  }
  </style>