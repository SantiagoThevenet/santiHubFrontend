<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-6 gap-10">
    <div class="col-span-5 flex flex-col gap-4">
      <div class="bg-white rounded-md border border-gray-200 p-10">
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

export default {
  components: {
    PeopleYouMayKnow,
    TrendsCard,
    PostText,
  },
  data() {
    return {
      posts: [],
      body: "",
    };
  },
  mounted() {
    this.getFeed();
  },
  methods: {
    getFeed() {
      axios
        .get("/api/posts/")
        .then(({ data }) => {
          console.log(data)
          this.posts = data;
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
