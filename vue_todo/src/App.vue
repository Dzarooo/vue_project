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

const newTodoText = ref("");

function updateNewTodoText(event) {
	newTodoText.value = event.target.value;
}

function addNewTodo() {
	if(newTodoText.value != "") {
		todos.value.push({
    	    name: newTodoText.value,
    	    tasks: [],
    	});

		warningsData.value.push({
			show:false,
			hidden:false,
		});
	}
}


function countFinished(tasks) {
	//console.log("test",tasks);
	const count = ref(tasks.filter(todo => todo.finished).length);
	//console.log(count.value);
	let result = "finished " + count.value + " of " + tasks.length + " tasks";
	return result;
}

function addNewTask(newTaskTextStatic, indexTodo) {
	console.log("app vue received: ", newTaskTextStatic, indexTodo);
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
		//console.log("index:", i);
		if(warningsData.value[i].hidden == false) {
			if(newTodo.tasks.length > 5) {
				warningsData.value[i].show = true;
			}
		}
		//console.log(warningsData.value);

	}, {deep:true, immediate:true});

};

function hideWarning(index) {
	//console.log(index);
	warningsData.value[index].hidden = true;
	warningsData.value[index].show = false;
}

function handleCheckboxToggle(indexItem, indexTodo) {
	//console.log(indexTodo, indexItem);
	//console.log(count)
	todos.value[indexTodo].tasks[indexItem].finished = !todos.value[indexTodo].tasks[indexItem].finished
}

const accordionComponents = ref();

function collapseAllTodos() {
	for(const accordion of accordionComponents.value) {
		console.log(accordion);
		accordion.closeWrapper();
	}
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
		<div style="background-color:rgb(39, 39, 39); border-radius:20px; padding:15px; margin-bottom:15px; display:flex; flex-direction:column; align-items:center">
			<button v-on:click="collapseAllTodos">Collapse all Todos</button>
			<h1>Add New Todo</h1>
			<div>
				<input v-on:input="updateNewTodoText" type="text" placeholder="New todo name" v-on:keypress.enter="addNewTodo">
				<button v-on:click="addNewTodo">Dodaj</button>
			</div>
		</div>
		<template v-for="(todo, indexTodo) in todos">
			<accordion v-bind:indexTodo="indexTodo" v-bind:todo="todo" v-bind:warningState="warningsData[indexTodo].show" v-on:created="addNewTask" v-on:hidden="hideWarning(indexTodo)" ref="accordionComponents">
				<template #header>
					<h1 style="text-align: center">{{ todo.name }}</h1>
					<p style="text-align:center">{{ countFinished(todo.tasks) }}</p>
				</template>
				<template v-for="(task, indexItem) in todo.tasks">
					<todo-item v-bind:task="task.text" v-bind:indexTodo="indexTodo" v-bind:indexItem="indexItem" v-model="task.finished" v-on:changed="handleCheckboxToggle"></todo-item>
				</template>
			</accordion>
		</template>


		<!--reactive name and computed() function-->
		<div style="display: flex; flex-direction: column; align-items: center; background-color:rgb(39, 39, 39); border-radius:20px; padding:5px">
			<p>{{ fullname }}</p>
			<input v-model="name" type="text">
			<input v-model="surname" type="text">
		</div>

	</div>
</template>