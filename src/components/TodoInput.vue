<script setup>
import { ref } from 'vue';

    const props = defineProps({
        indexTodo: {
            type: Number,
            required: true,
        },
    });

    const emits = defineEmits(['created']);


    const newTaskText = ref("");


    function updateNewTaskText(event) {
        newTaskText.value = event.target.value;
    }

    function addNewTask(indexTodo) {
        //console.log(newTaskText.value);
        //console.log(indexTodo);
        let newTaskTextStatic = newTaskText.value;
        if(newTaskTextStatic != "") {
		    emits("created", newTaskTextStatic, indexTodo);
	    }
    }
</script>

<template>
    <div style="margin-bottom:10px">
		<input v-on:input="updateNewTaskText" type="text" placeholder="New task" v-on:keypress.enter="addNewTask(props.indexTodo)">
		<button v-on:click="addNewTask(props.indexTodo)">Dodaj</button>
	</div>
</template>