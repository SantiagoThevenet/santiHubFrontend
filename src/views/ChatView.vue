<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-5 gap-10">
    <div
      class="col-span-1 bg-white rounded-md border border-gray-200 h-fit"
      v-for="conversation in conversations"
      :key="conversation.id"
    >
      <section class="flex justify-between gap-4 items-center p-4">
        <img
          class="rounded-full aspect-square object-cover w-14"
          src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
          alt=""
        />
        <template v-for="user in conversation.users" :key="user.id">
          <p class="text-xl font-semibold" v-if="user.id !== userStore.user.id">
            {{ user.name }}
          </p>
        </template>
        <span class="text-base text-center text-gray-700">
          {{ conversation.modified_at_formatted }}
        </span>
      </section>
    </div>

    <div class="col-span-4">
      <div
        class="flex flex-col gap-6 h-fit bg-white rounded-md border border-gray-200 p-4"
      >
        <template
          v-for="message in activeConversation.messages"
          :key="message.id"
        >
          <div
            class="flex self-end max-w-[80%] gap-4"
            v-if="message.created_by.id == userStore.user.id"
          >
            <div>
              <p
                class="text-neutral-50 bg-indigo-500 p-4 rounded-b-xl rounded-l-xl"
              >
                {{ message.body }}
              </p>
              <span class="text-sm text-gray-500"
                >{{ message.created_at_formatted }} ago</span
              >
            </div>
            <img
              class="rounded-full aspect-square object-cover h-14"
              src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
              alt=""
            />
          </div>
          <div class="flex max-w-[80%] gap-4" v-else>
            <img
              class="rounded-full aspect-square object-cover h-14"
              src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
              alt=""
            />
            <div>
              <p
                class="text-neutral-900 bg-gray-300 p-4 rounded-b-xl rounded-r-xl"
              >
                {{ message.body }}
              </p>
              <span class="text-sm text-gray-500"
                >{{ message.created_at_formatted }} ago</span
              >
            </div>
          </div>
        </template>
      </div>
      <div class="bg-white rounded-md border border-gray-200 p-10 mt-4">
        <form action="" v-on:submit.prevent="submitForm" class="flex flex-col">
          <textarea
            class="bg-gray-100 rounded-md p-4"
            placeholder="Write a post"
            v-model="body"
          ></textarea>
          <div class="flex mt-9 border-t border-gray-100 py-4">
            <button class="bg-indigo-500 rounded-md py-3 px-6 text-white">
              Send
            </button>
          </div>
        </form>
      </div>
    </div>
  </main>
</template>  

<script>
import axios from 'axios'
import { useUserStore } from '../stores/user'

export default {
  name: "chat",

  setup() {
    const userStore = useUserStore();

    return {
      userStore,
    };
  },

  data() {
    return {
      conversations: [],
      activeConversation: {},
      body: "",
    };
  },

  mounted() {
    this.getConversations();
  },

  methods: {
    getConversations() {

      axios
        .get("/api/chat/")
        .then((response) => {

          this.conversations = response.data;

          if (this.conversations.length) {
            this.activeConversation = this.conversations[0].id;
        }
            console.log(this.activeConversation)
            this.getMessages();
            console.log(this.activeConversation)
        })
        .catch((error) => {
          console.log(error);
        });
    },

    getMessages() {

      axios
        .get(`/api/chat/${this.activeConversation}/`)
        .then((response) => {
          this.activeConversation = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    submitForm() {
      console.log("submitForm", this.body);

      axios
        .post(`/api/chat/${this.activeConversation.id}/send/`, {
          body: this.body,
        })
        .then((response) => {
          console.log(response.data);

          this.activeConversation.messages.push(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style>
</style>