<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-6 gap-10">
    <div class="col-span-5 flex flex-col gap-4">
      <PostText v-if="posts.id" :postData="posts" />
      <div
        v-for="(comment, index) in posts.comments"
        :key="index"
      >
        <CommentItem :comment="comment"/>
      </div>

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
            <button class="bg-indigo-500 rounded-md py-3 px-6 text-white">
              Comment
            </button>
          </div>
        </form>
      </div>
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
import CommentItem from "../components/CommentItem.vue";

export default {
  components: {
    PeopleYouMayKnow,
    TrendsCard,
    PostText,
    CommentItem,
  },
  data() {
    return {
      posts: {
        id: null,
        comments: [],
      },
      body: "",
    };
  },
  mounted() {
    this.getPost();
  },
  methods: {
    getPost() {
      axios
        .get(`/api/posts/${this.$route.params.id}/`)
        .then(({ data }) => {
          console.log(data);
          this.posts = data.post;
        })
        .catch((error) => console.log("error", error));
    },
    submitForm() {
      console.log("submitForm", this.body);
      axios
        .post(`/api/posts/${this.$route.params.id}/comment/`, {
          body: this.body,
        })
        .then((response) => {
          console.log(response.data);
          if (Array.isArray(this.posts.comments)) {
            this.posts.comments.push(response.data);
            this.posts.comments_count += 1
          } else {
            this.posts.comments = [response.data];
          }

          this.body = "";
        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
  