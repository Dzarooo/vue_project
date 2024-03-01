<script setup>
import { ref, reactive } from 'vue';

const oldName = ref('Kacperek');
oldName.value = 'Kacper';

const user = reactive({
	name: "Kacper",
	surname: "Jaroszewski",
});
user.surname = "Nowak";

const colours = ["red", "green", "blue"];
const selectedColour = ref("not selected yet");

const updateName = (event) => {
	oldName.value = event.target.value;
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
			<h1>{{ oldName }}</h1>
			<input type="text" v-bind:value="oldName" v-on:input="updateName">
			<input type="text" v-model="oldName">

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
								{{ task.text }}
							</label>
						</li>
						<li v-key="index" v-else style="display:flex">
							<input type="checkbox" :id="'task' + index" v-model="todo[index].finished" style="margin-right:5px">
							<label :for="'task' + index" style="color:rgb(89, 89, 89); text-decoration-line: line-through">
								{{ task.text }}
							</label>
						</li>
					</template>
				</ul>
			</template>
			<p v-else>There are no tasks to do</p>
		</div>



	</div>
</template>