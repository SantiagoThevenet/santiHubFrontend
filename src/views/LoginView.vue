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
          <RouterLink to="/signup"
            ><b class="text-gray-800 underline">Click here</b></RouterLink
          >
          to create one!
        </p>
      </section>
      <section class="bg-white rounded-lg w-full border border-gray-200 p-8">
        <h1 class="text-3xl text-gray-900 font-bold mb-4">Log in</h1>
        <form
          v-on:submit.prevent="submitForm"
          class="flex flex-col gap-5 items-start"
          action=""
        >
          <input
            type="email"
            v-model="form.email"
            placeholder="Email"
            class="border rounded-md h-10 w-full pl-2"
          />
          <input
            type="password"
            v-model="form.password"
            placeholder="Password"
            class="border rounded-md h-10 w-full pl-2"
          />
          <template v-if="errors.length > 0">
            <div class="bg-red-300 text-white rounded-lg p-6 w-full">
              <p v-for="error in errors" :key="error">{{ error }}</p>
            </div>
          </template>

          <button
            class="bg-indigo-500 text-white rounded py-3 px-6 items-start"
          >
            Log in
          </button>
        </form>
      </section>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import { useUserStore } from "@/stores/user";

export default {
  setup() {
    const userStore = useUserStore();

    return {
      userStore,
    };
  },
  data() {
    return {
      form: {
        email: "",
        password: "",
      },
      errors: [],
    };
  },
  methods: {
    async submitForm() {
      this.errors = [];

      if (this.form.email === "") {
        this.errors.push("Your email is missing");
      }

      if (this.form.password === "") {
        this.errors.push("Your password is missing");
      }

      if (this.errors.length === 0) {
        await axios
          .post("/api/login/", this.form)
          .then((res) => {
            this.userStore.setToken(res.data);
            axios.defaults.headers.common["Authorization"] =
              "Bearer " + res.data.access;
          })
          .catch((err) => {
            console.log("error", err);
          });

        await axios
          .get("/api/me/")
          .then((request) => {
            this.userStore.setUserInfo(request.data);
            this.$router.push("/feed");
          })
          .catch((err) => {
            console.log("error", err);
          });
      }
    },
  },
};
</script>