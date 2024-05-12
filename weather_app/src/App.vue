<script setup>
  import Searchbar from './components/Searchbar.vue';
  import CityWeather from './components/CityWeather.vue';
  import temperatureUnit from './components/temperatureUnit.vue';
  import { ref } from 'vue';

  const searchData = ref("");
  const temperatureUnitData = ref("");

  function forwardSearchResult(searchQuery) {
    console.log("searchQuery received in App.vue:", searchQuery);
    searchData.value = searchQuery;
  }

  function forwardNewUnit(newUnit) {
    console.log("newUnit received in App.vue:", newUnit);
    temperatureUnitData.value = newUnit;
  }

</script>

<template>
  <div class="w-full min-h-[100vh] flex flex-col websiteGradient">
    <div class="flex justify-center mt-10 h-[70px] gap-5">
      <Searchbar v-on:searched="forwardSearchResult"/>
      <temperatureUnit v-on:unitChanged="forwardNewUnit"/>
    </div>
    <div class="flex-1 w-full flex flex-col h-[calc(100vh-110px)]">
      <CityWeather v-bind:searchQuery="searchData" v-bind:temperatureUnit="temperatureUnitData"/>
    </div>
  </div>
</template>
