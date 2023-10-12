<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-6 gap-10">
    <div class="col-span-5 flex flex-col gap-4">
      <div class="bg-white gap-6 p-4 grid border border-gray-200 rounded-md">
        <form v-on:submit.prevent="submitForm" class="flex gap-4" action="">
          <input
            v-model="query"
            type="text"
            class="bg-gray-100 rounded-md w-full pl-4"
            placeholder="What are you looking for?"
          />
          <button class="bg-indigo-500 rounded-md py-3 px-6 text-white">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z"
              />
            </svg>
          </button>
        </form>
      </div>

      <div
        v-if="users.length"
        class="bg-white gap-6 p-4 grid grid-cols-4 border border-gray-200 rounded-md"
      >
        <div
          v-for="(user, index) in users"
          class="flex flex-col gap-4 text-center bg-gray-100 rounded-md p-8"
          :key="index"
        >
          <img
            class="rounded-full aspect-square object-cover"
            src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
            alt=""
          />
          <RouterLink
            :to="{ name: 'profile', params: { id: user.id } }"
            class="text-2xl font-bold text-gray-900"
            >{{ user.name }}</RouterLink
          >
          <div class="flex justify-between">
            <span class="text-base text-gray-700">1000 <br />followers</span>
            <span class="text-base text-gray-700"
              >23 <br />
              post</span
            >
          </div>
        </div>
      </div>
      <PostText v-for="(item, index) in posts" :key="index" :postData="item" />
    </div>

    <div class="col-span-1 flex flex-col gap-4">
      <PeopleYouMayKnow />
      <div class="bg-white rounded-md border border-gray-200">
        <TrendsCard />
      </div>
    </div>
  </main>
</template>


<script>
import axios from "axios";
import PeopleYouMayKnow from "../components/PeopleYouMayKnow.vue";
import TrendsCard from "../components/TrendsCard.vue";
import PostText from "../components/PostText.vue";
import { RouterLink } from "vue-router";

export default {
  components: {
    TrendsCard,
    PeopleYouMayKnow,
    PostText,
    RouterLink,
  },
  data() {
    return {
      query: "",
      users: [],
      posts: [],
    };
  },
  methods: {
    submitForm() {
      axios
        .post("/api/search/", {
          query: this.query,
        })
        .then(({ data }) => {
          console.log("data", data.users);
          this.users = data.users;
          this.posts = data.posts;
        })
        .catch((err) => {
          console.log("error", err);
        });
    },
  },
};
</script>
