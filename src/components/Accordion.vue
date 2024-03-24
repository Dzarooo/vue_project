<script setup>
    import TodoItem from './TodoItem.vue';
    import TodoInput from './TodoInput.vue';

    const props = defineProps({
        indexTodo: {
            type: Number,
            required: true
        },
        todo: {
            type:Object,
            required:true,
        },
        warningState: {
            type:Boolean,
            required:true,
        }
    });

    const emits = defineEmits([
        'changed',
        'created',
        'hidden',
    ]);


    function toggleWrapper(wrapperId) {
        let wrapper = document.getElementById("wrapper" + wrapperId);
        //let arrow = document.getElementById("arrow" + wrapperId);

        if(wrapper.style.gridTemplateRows == "0fr") {
            wrapper.style.gridTemplateRows = "1fr";
        }
        else {
            wrapper.style.gridTemplateRows = "0fr";
        }

        //console.log(props.todo);

        // if (wrapper.classList.contains("[grid-template-rows:1fr]")) {
        //     arrow.style.transform = "";
        // } else {
        //     arrow.style.transform = "rotate(180deg)";
        // }
    }

    function addNewTask(newTaskTextStatic, indexTodo) {
        //console.log("accordion received:", newTaskTextStatic, indexTodo);
        emits('created', newTaskTextStatic, indexTodo);
    }

    function hideWarning(indexTodo) {
        emits('hidden', indexTodo);
    }
</script>



<template>
    <div style="background-color:rgb(39, 39, 39); border-radius:20px; padding:15px; margin-bottom:15px">

        <div v-on:click="toggleWrapper(props.indexTodo)"><slot name="header"></slot></div>

        
        <div :id="'wrapper'+props.indexTodo"  style="display: grid; grid-template-rows: 0fr; overflow:hidden; transition-property:all; transition-duration:300ms;">
            <div style="overflow:hidden; display:flex; flex-direction: column; align-items: center;">

                <!-- <p>{{ finishedTasksOfAll }}</p> -->
                <todo-input v-on:created="addNewTask" v-bind:indexTodo="props.indexTodo"></todo-input>

                <template v-if="todo.tasks.length > 0">
					<ul>
                        <slot>
                        </slot>
					</ul>
				</template>

                <!-- <p style="margin-top:10px;">{{ progressText }}</p>
				<p v-if="finishedAll && todo.length > 0">All tasks done!</p> -->

                <div v-show="props.warningState" style="color:rgb(255, 62, 62); display:flex; flex-direction: column; align-items: center; margin-top:10px;">
		        	<p>Instead of adding new tasks, do existing ones!</p>
		        	<button v-on:click="hideWarning(indexTodo)">OK</button>
		        </div>

            </div>
        </div>

    </div>
</template>