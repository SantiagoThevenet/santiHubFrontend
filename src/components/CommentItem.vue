<template>
    <div class="ml-6  bg-white rounded-md border border-gray-200">
      <section class="flex justify-between items-center p-4">
        <div class="flex items-center gap-6">
          <img
            class="rounded-full aspect-square object-cover w-20"
            src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
            alt=""
          />
          <RouterLink
            :to="{ name: 'profile', params: { id: comment.created_by?.id } }"
            class="text-2xl font-bold text-gray-900"
            >{{ comment.created_by?.name }}</RouterLink
          >
        </div>
        <span class="text-xl text-gray-700">{{
          comment.created_at_formatted
        }}</span>
      </section>
      <p class="p-4 text-xl">
        {{ comment.body }}
      </p>
      
    </div>
  </template>
  
  <script>
  import { RouterLink } from "vue-router";
  export default {
    props: {
      comment: Object
    },
    components: { RouterLink },
  
  };
  </script>