<script setup>
import { ref, reactive, computed, watch, onMounted } from 'vue';

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
]);

const showWarning = ref(false);
let finishedTasksOfAll = computed(() => "finished " + todo.value.filter(newTodo => newTodo.finished).length + " out of " + todo.value.length + " tasks");
const progressText = ref("error occurred");
const finishedAll = computed(() => todo.value.every(task => task.finished)); //check if all tasks are finished, returns true or false
let watchActive = true; //watch is deactivated once the user clicks "ok" button in warning

function hideWarning() {
	showWarning.value = false;
}

function addNewTask() {
	const newTaskText = document.getElementById("newTaskText").value;
	if(newTaskText != "") {
		todo.value.push({text: newTaskText, finished: false});
	}
}

watch(todo, (newTodo) => {
	if(watchActive) {
		if(newTodo.length > 5) {
			showWarning.value = true;
			watchActive = false;
		}
	}
	console.log(newTodo.filter(newTodo => newTodo.finished).length + 3);
	console.log(newTodo.length);
	if(newTodo.filter(newTodo => newTodo.finished).length == 0) {
		progressText.value = "Time to get some work done";
	}
	else if(newTodo.filter(newTodo => newTodo.finished).length == newTodo.length) {
		progressText.value = "All done!";
	}
	else if(newTodo.filter(newTodo => newTodo.finished).length + 3 >= newTodo.length) {
		progressText.value = "Almost done!";
	}
	else if(newTodo.filter(newTodo => newTodo.finished).length > 0) {
		progressText.value = "It's all systems go";
	}
}, {deep:true, immediate:true});



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
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px; margin-bottom:15px">
			<h1>TODO LIST</h1>
			<p>{{ finishedTasksOfAll }}</p>
			<div style="margin-bottom:10px">
				<input id="newTaskText" type="text" placeholder="New task" v-on:keypress.enter="addNewTask">
				<button v-on:click="addNewTask">Dodaj</button>
			</div>
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
			<p style="margin-top:10px;">{{ progressText }}</p>
			<p v-if="finishedAll && todo.length > 0">All tasks done!</p>
			<div v-show="showWarning" style="color:rgb(255, 62, 62); display:flex; flex-direction: column; align-items: center; margin-top:10px;">
				<p>Instead of adding new tasks, do existing ones!</p>
				<button v-on:click="hideWarning">OK</button>
			</div>
		</div>


		<!--reactive name and computed() function-->
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px">
			<p>{{ fullname }}</p>
			<input v-model="name" type="text">
			<input v-model="surname" type="text">
		</div>

	</div>
</template>