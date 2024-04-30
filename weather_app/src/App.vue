<script setup>
  import Searchbar from './components/Searchbar.vue'

  function forwardSearchResult(searchResult) {
    console.log("i received data in App.vue");
  }

</script>

<template>
  <div class="w-full min-h-[100vh]">
    <div class="flex justify-center mt-10">
      <Searchbar v-on:searched="forwardSearchResult"/>
    </div>
  </div>
</template>
