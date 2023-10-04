<template>
    <main class="px-8 bg-gray-100">
    <div class="flex justify-between gap-10 py-10">
      <section
        class="bg-white rounded-lg w-full border border-gray-200 flex flex-col gap-6 p-8"
      >
        <h1 class="text-3xl text-gray-900 font-bold">Sign up</h1>
        <p class="text-gray-800 text-base">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Est error hic
          ipsum perferendis, ullam eaque officiis ea nisi voluptatem adipisci,
          laudantium odio, delectus maxime nulla facere quos consequatur
          deleniti numquam?
        </p>
        <p class="text-gray-700 text-sm">
          Don't have an account?
          <b class="text-gray-800 underline">Click here</b> to create one!
        </p>
      </section>
      <section class="bg-white rounded-lg w-full border border-gray-200 p-8">
        <h1 class="text-3xl text-gray-900 font-bold mb-4">Log in</h1>
        <form class="flex flex-col  gap-5 items-start" action="">
          <input type="email" placeholder="Email" class="border rounded-md h-10 w-full pl-2" />
          <input type="password" placeholder="Password" class="border rounded-md h-10 w-full pl-2"/>
          <button class="bg-indigo-500 text-white rounded py-3 px-6 items-start">
            Log in
          </button>
        </form>
      </section>
    </div>
  </main>
</template>

<script>
export default {

}
</script>

<style>

</style>