<script setup>
import { ref, reactive, computed, watch, onMounted, warn } from 'vue';
import TodoItem from './components/TodoItem.vue'; 
import TodoInput from './components/TodoInput.vue';
import Accordion from './components/Accordion.vue';

//reactive name
const oldName = ref('Kacperek');
oldName.value = 'Kacper';

const user = reactive({
	name: "Kacper",
	surname: "Jaroszewski",
});
user.surname = "Nowak";

const updateName = (event) => {
	oldName.value = event.target.value;
}

//colours
const colours = ["red", "green", "blue"];
const selectedColour = ref("not selected yet");

//todo
const todos = ref([
		{
			name: "Todo1",
			tasks: [
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
				},
				{
					text: 'Spakować się do szkoły',
					finished: false,
				},
				{
					text: 'Nauczyć się na sprawdzian z polskiego',
					finished: false,
				},
				{
					text: 'wstawić pranie',
					finished: false,
				},
			]
		},
		{
			name: "Todo2",
            tasks: [
				{
					text: 'odrobić polski',
					finished: false,
				},
				{
					text: 'zrobić vue',
					finished: false,
				},
				{
					text: 'kupić prezent na 18-nastkę',
					finished: true,
				},
				{
					text: 'Dokonanie przewrotu w państwie',
					finished: false,
				},
				{
					text: 'zrewolucjonizować przemysł zbrojeniowy',
					finished: false,
				},
				{
					text: 'nauczyć się klasy shape w godocie',
					finished: false,
				},
			]
		}
]);

//let finishedTasksOfAll = computed(() => "finished " + todo.value.filter(newTodo => newTodo.finished).length + " out of " + todo.value.length + " tasks");
//const progressText = ref("error occurred");
//const finishedAll = computed(() => todo.value.every(task => task.finished)); //check if all tasks are finished, returns true or false

function addNewTask(newTaskTextStatic, indexTodo) {
	//console.log(indexTodo);
	todos.value[indexTodo].tasks.push({text: newTaskTextStatic, finished: false});
}

const warningsData = ref([]);

for(let i = 0; i < todos.value.length; i++) {
	warningsData.value.push({
		show:false,
		hidden:false,
	});
}

for(let i = 0; i < todos.value.length; ++i) {
	let todo = todos.value[i];
	watch(todo, (newTodo) => {
		console.log("index:", i);
		if(warningsData.value[i].hidden == false) {
			if(newTodo.tasks.length > 5) {
				warningsData.value[i].show = true;
			}
		}
		console.log(warningsData.value);

	}, {deep:true, immediate:true});

};

function hideWarning(index) {
	console.log(index);
	warningsData.value[index].hidden = true;
	warningsData.value[index].show = false;
}

// watch(todo, (newTodo) => {
// 	if(watchActive) {
// 		if(newTodo.length > 5) {
// 			showWarning.value = true;
// 			watchActive = false;
// 		}
// 	}
// 	// console.log(newTodo.filter(newTodo => newTodo.finished).length + 3);
// 	// console.log(newTodo.length);
// 	if(newTodo.filter(newTodo => newTodo.finished).length == 0) {
// 		progressText.value = "Time to get some work done";
// 	}
// 	else if(newTodo.filter(newTodo => newTodo.finished).length == newTodo.length) {
// 		progressText.value = "All done!";
// 	}
// 	else if(newTodo.filter(newTodo => newTodo.finished).length + 3 >= newTodo.length) {
// 		progressText.value = "Almost done!";
// 	}
// 	else if(newTodo.filter(newTodo => newTodo.finished).length > 0) {
// 		progressText.value = "It's all systems go";
// 	}
// }, {deep:true, immediate:true});

function handleCheckboxToggle(indexItem, indexTodo) {
	//console.log(indexTodo, indexItem);
	todos.value[indexTodo].tasks[indexItem].finished = !todos.value[indexTodo].tasks[indexItem].finished
}



//reactive name and computed() function
const name = ref('Kacper');
const surname = ref('Jaroszewski');
const fullname = computed(() => name.value + " " + surname.value);	

</script>

<template>
	<div>
		<!--reactive name-->
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:15px">
			<h1>{{ oldName }}</h1>
			<input type="text" v-bind:value="oldName" v-on:input="updateName">
			<input type="text" v-model="oldName">
			
			<ul>
				<li>{{ user.name }}</li>
				<li> {{ user.surname }}</li>
			</ul>
		</div>

		<!--colours-->
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:15px">
			<h1>CHOOSE COLOUR!</h1>
			<select v-model="selectedColour">
				<option active hidden>Select colour</option>
				<option v-for="colour in colours">{{ colour }}</option>
			</select>

			<p>chosen colour: <span :style="{ color: selectedColour }">{{ selectedColour }}</span></p>
		</div>


		<!--todo-->
		<template v-for="(todo, indexTodo) in todos">
			<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:15px">

				<h1>{{ todo.name }}</h1>
				<!-- <p>{{ finishedTasksOfAll }}</p> -->
				<todo-input v-on:created="addNewTask" v-bind:indexTodo="indexTodo"></todo-input>

				<template v-if="todo.tasks.length > 0">
					<ul>
						<template v-for="(task, indexItem) in todo.tasks">
							<todo-item v-bind:task="task.text" v-bind:indexTodo="indexTodo" v-bind:indexItem="indexItem" v-model="task.finished" v-on:changed="handleCheckboxToggle"></todo-item>
						</template>
					</ul>
				</template>

				<!-- <p style="margin-top:10px;">{{ progressText }}</p>
				<p v-if="finishedAll && todo.length > 0">All tasks done!</p> -->

				<div v-show="warningsData[indexTodo].show" style="color:rgb(255, 62, 62); display:flex; flex-direction: column; align-items: center; margin-top:10px;">
					<p>Instead of adding new tasks, do existing ones!</p>
					<button v-on:click="hideWarning(indexTodo)">OK</button>
				</div>

			</div>
		</template>
		<accordion></accordion>


		<!--reactive name and computed() function-->
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px">
			<p>{{ fullname }}</p>
			<input v-model="name" type="text">
			<input v-model="surname" type="text">
		</div>

	</div>
</template>