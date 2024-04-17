<script setup>
  import GitHubCardWrapper from './components/GitHubCardWrapper.vue';
  import { ref } from 'vue';

  const users = [
    "MattMoszczynski",
    "Dzarooo",
    "JustJuulia",
    "ajhdbjhasdbquwgdyuasbjhd",
    "SzymonDrzazga"
  ]

  const search = ref("");
  const searchToProp = ref("");

  function searchUser() {
    searchToProp.value = search.value;
    console.log(searchToProp.value);
  }
</script>

<template>
  <div class="min-w-[100vw]">
    <GitHubCardWrapper v-bind:users="users" v-bind:searchUsers="searchToProp"></GitHubCardWrapper>
  </div>
  <div class="w-full flex flex-col items-center py-10 px-2">
    <div class="flex gap-[10px]">
    <input placeholder="Search" v-model="search" class="w-[500px] border-solid border-b-[2px] border-b-gray-300 focus:border-b-gray-500 outline-none"> 
    <div v-on:click="searchUser()" class="cursor-pointer"><i class="bi bi-search text-gray-400 hover:text-gray-700"></i></div>
    </div>
  </div>
</template>