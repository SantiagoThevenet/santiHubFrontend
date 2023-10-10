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
        <span class="font-bold text-gray-900">Pepito</span>
        <div class="flex justify-between">
          <span class="text-sm text-gray-800">1000 followers</span>
          <span class="text-sm text-gray-800">23 post</span>
        </div>
      </div>
    </div>
    <div class="col-span-4 flex flex-col gap-4">
      <div class="bg-white rounded-md border border-gray-200 p-10">
        <form action="" class="flex flex-col">
          <textarea
            class="bg-gray-100 rounded-md p-4"
            placeholder="Write a post"
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
      <div class="bg-white rounded-md border border-gray-200">
        <section class="flex justify-between items-center p-4">
          <div class="flex items-center gap-6">
            <img
              class="rounded-full aspect-square object-cover w-20"
              src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
              alt=""
            />
            <span class="text-3xl font-semibold">Santiago</span>
          </div>
          <span class="text-xl text-gray-700">18 minutes ago</span>
        </section>
        <img
          class="w-full rounded-md aspect-video object-cover"
          src="https://images.pexels.com/photos/2662116/pexels-photo-2662116.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
          alt=""
        />
        <div class="flex justify-between py-8 px-4">
          <div class="flex gap-8">
            <div class="flex items-center gap-2">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-8 h-8"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z"
                />
              </svg>
              <span class="text-gray-700">82 likes</span>
            </div>
            <div class="flex items-center gap-2">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-8 h-8"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M12 20.25c4.97 0 9-3.694 9-8.25s-4.03-8.25-9-8.25S3 7.444 3 12c0 2.104.859 4.023 2.273 5.48.432.447.74 1.04.586 1.641a4.483 4.483 0 01-.923 1.785A5.969 5.969 0 006 21c1.282 0 2.47-.402 3.445-1.087.81.22 1.668.337 2.555.337z"
                />
              </svg>
              <span class="text-gray-700">0 comments</span>
            </div>
          </div>
          <button>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 6.75a.75.75 0 110-1.5.75.75 0 010 1.5zM12 12.75a.75.75 0 110-1.5.75.75 0 010 1.5zM12 18.75a.75.75 0 110-1.5.75.75 0 010 1.5z"
              />
            </svg>
          </button>
        </div>
      </div>
      <PostText v-for="item,index in posts" :key="index" :postData=item />
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
import axios from 'axios';
import PeopleYouMayKnow from "../components/PeopleYouMayKnow.vue";
import TrendsCard from "../components/TrendsCard.vue";
import PostText from "../components/PostText.vue";

export default { 
    components: { 
        PeopleYouMayKnow, 
        TrendsCard, 
        PostText 
    },
    data() {
        return {
            posts: []
        }
    },  
    mounted() {
        this.getFeed() 
    },
    methods: {
        getFeed() {
            axios.get('/api/posts/')
            .then(({data}) => {
                this.posts = data
                console.log(data[0])
            })
            .catch(error => {
                console.log('error', error)
            })
        }
    }

};

</script>
