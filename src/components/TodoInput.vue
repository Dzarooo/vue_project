<script setup>
    const emits = defineEmits(['created']);

    function addNewTask() {
        const newTaskText = document.getElementById("newTaskText").value;
        if(newTaskText != "") {
		    emits("created", newTaskText);
	    }
    }
</script>

<template>
    <div style="margin-bottom:10px">
		<input id="newTaskText" type="text" placeholder="New task" v-on:keypress.enter="addNewTask">
		<button v-on:click="addNewTask">Dodaj</button>
	</div>
</template>