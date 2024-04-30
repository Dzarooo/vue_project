<script setup>
  import Searchbar from './components/Searchbar.vue';
  import CityWeather from './components/CityWeather.vue';
  import { ref } from 'vue';

  let searchData = "";

  function forwardSearchResult(searchQuery) {
    console.log("data received in App.vue");
    console.log(searchQuery);
    searchData = searchQuery;
  }

</script>

<template>
  <div class="w-full min-h-[100vh] flex flex-col websiteGradient">
    <div class="flex justify-center mt-10">
      <Searchbar v-on:searched="forwardSearchResult"/>
    </div>
    <div class="flex-1 w-full flex flex-col">
      <CityWeather v-bind:searchQuery="searchData"/>
    </div>
  </div>
</template>
