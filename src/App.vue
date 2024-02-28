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

const todo = ref([
	{
		text: 'Zrobić matmę',
		finished: false,
	},
	{
		text: 'Odkurzyć pokój',
		finished: false,
	},
	{
		text: 'Wynieść śmieci',
		finished: true,
	}
]);

</script>

<template>
	<div>

		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:5px">
			<h1>{{ name }}</h1>
			<input type="text" v-bind:value="name" v-on:input="updateName">
			<input type="text" v-model="name">

			<ul>
				<li>{{ user.name }}</li>
				<li> {{ user.surname }}</li>
			</ul>
		</div>

		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:5px">
			<h1>CHOOSE COLOUR!</h1>
			<select v-model="selectedColour">
				<option active hidden>Select colour</option>
				<option v-for="colour in colours">{{ colour }}</option>
			</select>

			<p>chosen colour: <span :style="{ color: selectedColour }">{{ selectedColour }}</span></p>
		</div>

		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px">
			<h1>TODO LIST</h1>
			<template v-if="todo.length > 0">
			<ul>
					<template v-for="(task, index) in todo">
						<li v-key="index" v-if="task.finished == false" style="display:flex">
							<input type="checkbox" :id="'task' + index" v-model="todo[index].finished" style="margin-right:5px">
							<label :for="'task' + index">
								{{ task.text }} {{ task.finished }}
							</label>
						</li>
						<li v-key="index" v-else style="display:flex">
							<input type="checkbox" :id="'task' + index" v-model="todo[index].finished" style="margin-right:5px">
							<label :for="'task' + index" style="color:rgb(89, 89, 89); text-decoration-line: line-through">
								{{ task.text }} {{ task.finished }}
							</label>
						</li>
					</template>
				</ul>
			</template>
			<p v-else>There are no tasks to do</p>
		</div>

	</div>
</template>