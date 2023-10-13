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
          <span class="text-sm text-gray-800"
            >{{ user.friends_count }} followers</span
          >
          <span class="text-sm text-gray-800">23 post</span>
        </div>
      </div>
    </div>
    <div
      v-if="friendshipRequests.length"
      class="col-span-4 flex flex-col gap-4 rounded-md bg-white p-4 border border-gray-200"
    >
      <h1 class="text-2xl font-bold">Frendship request</h1>
      <div class="gap-6 grid grid-cols-4">
        <div
          v-for="(friend, index) in friendshipRequest"
          class="flex flex-col gap-4 text-center bg-gray-100 rounded-md p-4"
          :key="index"
        >
          <img
            class="rounded-full aspect-square object-cover"
            src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
            alt=""
          />
          <RouterLink
            :to="{ name: 'profile', params: { id: friend.created_by.id } }"
            class="text-2xl font-bold text-gray-900"
            >{{ friend.created_by.name }}</RouterLink
          >
          <div class="flex justify-between">
            <span class="text-base text-gray-700"
              >{{ friend.created_by.friends_count }} <br />followers</span
            >
            <span class="text-base text-gray-700"
              >23 <br />
              post</span
            >
          </div>
          <div class="flex justify-between">
            <button
              @click="handleRequest('accept', friend.created_by.id)"
              class="bg-indigo-500 rounded-md py-2 px-5 text-white"
            >
              Accept
            </button>
            <button
              @click="handleRequest('reject', friend.created_by.id)"
              class="bg-red-500 rounded-md py-2 px-5 text-white"
            >
              Reject
            </button>
          </div>
        </div>
      </div>
      <hr />
      <div v-if="friends.length" class="flex flex-col gap-4 p-4">
        <h1 class="text-2xl font-bold">Friends</h1>
        <div class="gap-6 grid grid-cols-4">
          <div
            v-for="(friend, index) in friends"
            class="flex flex-col gap-4 text-center bg-gray-100 rounded-md p-4"
            :key="index"
          >
            <img
              class="rounded-full aspect-square object-cover"
              src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
              alt=""
            />
            <RouterLink
              :to="{ name: 'profile', params: { id: friend.id } }"
              class="text-2xl font-bold text-gray-900"
              >{{ friend.name }}</RouterLink
            >
            <div class="flex justify-between">
              <span class="text-base text-gray-700"
                >{{ friend.friends_count }} <br />followers</span
              >
              <span class="text-base text-gray-700"
                >23 <br />
                post</span
              >
            </div>
          </div>
        </div>
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
import { useUserStore } from "@/stores/user";

export default {
  name: "FriendsView",

  setup() {
    const userStore = useUserStore();

    return {
      userStore,
    };
  },

  components: {
    PeopleYouMayKnow,
    TrendsCard,
  },

  data() {
    return {
      user: {},
      friendshipRequests: [],
      friends: [],
    };
  },

  mounted() {
    this.getFriends();
  },

  methods: {
    getFriends() {
      axios
        .get(`/api/friends/${this.$route.params.id}/`)
        .then((response) => {
          console.log(response.data)
          this.friendshipRequests = response.data.requests;
          this.friends = response.data.friends;
          this.user = response.data.user;
        })
        .catch((error) => {
          console.log("error", error);
        });
    },

    handleRequest(status, pk) {
      console.log("handleRequest", status);

      axios
        .post(`/api/friends/${pk}/${status}/`)
        .then((response) => {
          console.log("data", response.data);
        })
        .catch((error) => {
          console.log("error", error);
        });
    },
  },
};
</script>
    