<template>
  <main class="px-8 bg-gray-100">
    <div class="flex justify-between gap-10 py-10">
      <section
        class="bg-white rounded-lg w-full border border-gray-200 flex flex-col gap-6 p-8 h-fit"
      >
        <h1 class="text-3xl text-gray-900 font-bold">Sign up</h1>
        <p class="text-gray-800 text-base">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Est error hic
          ipsum perferendis, ullam eaque officiis ea nisi voluptatem adipisci,
          laudantium odio, delectus maxime nulla facere quos consequatur
          deleniti numquam?
        </p>
        <p class="text-gray-700 text-sm">
          Already have an account?
          <RouterLink to="/login"
            ><b class="text-gray-800 underline">Click here</b></RouterLink
          >
          to log in!
        </p>
      </section>
      <section class="bg-white rounded-lg w-full border border-gray-200 p-8">
        <h1 class="text-3xl text-gray-900 font-bold mb-4">Log in</h1>
        <form
          class="flex flex-col gap-5 items-start"
          v-on:submit.prevent="submitForm"
        >
          <input
            type="text"
            v-model="form.name"
            placeholder="Username"
            class="border rounded-md h-10 w-full pl-2"
          />
          <input
            type="email"
            v-model="form.email"
            placeholder="Email"
            class="border rounded-md h-10 w-full pl-2"
          />
          <input
            type="password"
            v-model="form.password1"
            placeholder="Password"
            class="border rounded-md h-10 w-full pl-2"
          />
          <input
            type="password"
            v-model="form.password2"
            placeholder="Repeat password"
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
            Sign up
          </button>
        </form>
      </section>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import { useToastStore } from "@/stores/toast";
export default {
  setup() {
    const toastStore = useToastStore();

    return {
      toastStore,
    };
  },

  data() {
    return {
      form: {
        email: "",
        name: "",
        password1: "",
        password2: "",
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

      if (this.form.name === "") {
        this.errors.push("Your name is missing");
      }

      if (this.form.password1 === "") {
        this.errors.push("Your password is missing");
      }

      if (this.form.password1 !== this.form.password2) {
        this.errors.push("Your password does not match");
      }
      if (this.errors.length === 0) {
        await axios
          .post("/api/signup/", this.form)
          .then((res) => {
            if (res.data.status === "success") {
              this.toastStore.showToast(
                5000,
                "The user is registered. Please log in",
                "bg-emerald-500"
              );
              this.form.email = "";
              this.form.name = "";
              this.form.password1 = "";
              this.form.password2 = "";
            } else {
              this.toastStore.showToast(
                5000,
                "Something went wrong. Please try angain",
                "bg-red-300"
              );
            }
          })
          .catch((err) => {
            console.log("error", err);
          });
      }
    },
  },
};
</script>