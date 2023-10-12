<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-6 gap-10">
    <div class="col-span-1">
      <div
        class="flex flex-col gap-4 text-center bg-white rounded-md border border-gray-200 p-8"
      >
        <img
          class="rounded-full aspect-square object-cover"
          src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
          alt=""
        />
        <span class="font-bold text-gray-900">{{ user.name }}</span>
        <div class="flex justify-between">
          <span class="text-sm text-gray-800">1000 followers</span>
          <span class="text-sm text-gray-800">23 post</span>
        </div>
      </div>
    </div>
    <div class="col-span-4 flex flex-col gap-4">
      <div v-if="userStore.user.id === user.id" class="bg-white rounded-md border border-gray-200 p-10">
        <form
          class="flex flex-col"
          v-on:submit.prevent="submitForm"
          method="POST"
        >
          <textarea
            class="bg-gray-100 rounded-md p-4"
            placeholder="Write a post"
            v-model="body"
          ></textarea>
          <div class="flex justify-between mt-9 border-t border-gray-100 py-4">
            <button class="bg-gray-500 rounded-md py-3 px-6 text-white">
              Attach image
            </button>
            <button class="bg-indigo-500 rounded-md py-3 px-6 text-white">
              Post
            </button>
          </div>
        </form>
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
import { useUserStore } from "@/stores/user";
export default {
  setup() {
    const userStore = useUserStore();
    return {
      userStore,
    };
  },
  components: {
    PeopleYouMayKnow,
    TrendsCard,
    PostText,
  },
  data() {
    return {
      posts: [],
      user: {}, 
      body: "",
    };
  },
  mounted() {
    this.getFeed();
  },
  watch: {
    '$route.params.id': {
      handler: function() {
        this.getFeed()
      },
      deep: true,
      immediate: true
    }
  },
  methods: {
    getFeed() {
      axios
        .get(`/api/posts/profile/${this.$route.params.id}/`)
        .then(({ data }) => {
          this.posts = data.posts;
          this.user = data.user
        })
        .catch((error) => console.log("error", error));
    },
    submitForm() {
      axios
        .post("/api/posts/create/", { body: this.body })
        .then((response) => {
          this.posts.unshift(response.data);
          this.body = "";
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
  