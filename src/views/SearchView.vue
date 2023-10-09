
import PeopleYouMayKnow from '../components/PeopleYouMayKnow.vue';
<template>
  <main class="px-[10%] py-8 bg-gray-100 grid grid-cols-6 gap-10">
    <div class="col-span-5 flex flex-col gap-4">
        <div class="bg-white gap-6 p-4 grid  border border-gray-200 rounded-md">
            <form class="flex gap-4" action="">
                <input type="text" class="bg-gray-100 rounded-md w-full pl-4" placeholder="What are you looking for?">
                <button class="bg-indigo-500 rounded-md py-3 px-6 text-white">
                  Search
                </button>
            </form>
        </div>

      <div  class="bg-white gap-6 p-4 grid grid-cols-4 border border-gray-200 rounded-md">
        <div
          v-for="item in [1,2,3,4,5,6,7]"
          class="flex flex-col gap-4 text-center bg-gray-100 rounded-md p-8"
          :key="item"
        >
          <img
            class="rounded-full aspect-square object-cover"
            src="https://images.pexels.com/photos/220453/pexels-photo-220453.jpeg?cs=srgb&dl=pexels-pixabay-220453.jpg&fm=jpg"
            alt=""
          />
          <span class="text-2xl font-bold text-gray-900">Pepito</span>
          <div class="flex justify-between">
            <span class="text-base text-gray-700">1000 <br>followers</span>
            <span class="text-base text-gray-700">23 <br> post</span>
          </div>
        </div>
      </div>
      <PostText />
    </div>

    <div class="col-span-1 flex flex-col gap-4">
      <PeopleYouMayKnow/>
      <div class="bg-white rounded-md border border-gray-200">
        <TrendsCard/>
      </div>
    </div>
  </main>
</template>


<script>
import PeopleYouMayKnow from '../components/PeopleYouMayKnow.vue';
import TrendsCard from '../components/TrendsCard.vue';
import PostText from '../components/PostText.vue';

export default { components: { PeopleYouMayKnow, TrendsCard, PostText } };
</script>
