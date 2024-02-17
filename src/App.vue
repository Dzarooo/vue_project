<script setup>
import { ref, reactive } from 'vue';  
  
  const name = ref('Kacperek');
  name.value = 'Kacper';
  
  const user = reactive({
    name: "Kacper",
    surname: "Jaroszewski",
  });
  user.surname = "Nowak";

  const colours = ["red", "green", "blue"];
  const selectedColour = ref("not selected yet");
  
  const updateName = (event) => {
    name.value = event.target.value;
  }

</script>

<template>
  <h1>{{ name }}</h1><br>
  <input type="text" v-bind:value="name" v-on:input="updateName"><br>
  <input type="text" v-model="name"><br>

  <ul>
    <li>{{ user.name }}</li>
    <li> {{ user.surname }}</li>
  </ul>
  <br>

  <select v-model="selectedColour">
    <option active hidden>Select colour</option>
    <option v-for="colour in colours">{{ colour }}</option>
  </select>
  <br>

  <p>chosen colour: <span :style="{ color: selectedColour }">{{ selectedColour }}</span></p>
</template>