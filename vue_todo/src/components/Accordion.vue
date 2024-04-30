<script setup>
    import TodoItem from './TodoItem.vue';
    import TodoInput from './TodoInput.vue';
    import { ref, watch } from 'vue';

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


    function toggleWrapper() {
        let wrapper = document.getElementById("wrapper" + props.indexTodo);
        let arrow = document.getElementById("arrow" + props.indexTodo);

        if(wrapper.style.gridTemplateRows == "0fr") {
            wrapper.style.gridTemplateRows = "1fr";
            arrow.style.transform = "";
        }
        else {
            wrapper.style.gridTemplateRows = "0fr";
            arrow.style.transform = "rotate(180deg)";
        }

        //console.log(props.todo);
    }

    function closeWrapper() {
        let wrapper = document.getElementById("wrapper" + props.indexTodo);
        let arrow = document.getElementById("arrow" + props.indexTodo);
        wrapper.style.gridTemplateRows = "0fr";
        arrow.style.transform = "rotate(180deg)";
        
        //console.log(props.todo);
    }

    function addNewTask(newTaskTextStatic, indexTodo) {
        //console.log("accordion received:", newTaskTextStatic, indexTodo);
        emits('created', newTaskTextStatic, indexTodo);
    }

    function hideWarning(indexTodo) {
        emits('hidden', indexTodo);
    }

    const progressText = ref("");
    watch(props.todo, (newTodo) => {
        console.log(newTodo.tasks);
    	if(newTodo.tasks.filter(todo => todo.finished).length == 0) {
    		progressText.value = "Time to get some work done";
    	}
    	else if(newTodo.tasks.filter(todo => todo.finished).length == newTodo.tasks.length) {
    		progressText.value = "All tasks done!";
    	}
    	else if(newTodo.tasks.filter(todo => todo.finished).length + 3 >= newTodo.tasks.length) {
    		progressText.value = "Almost done!";
    	}
    	else if(newTodo.tasks.filter(todo => todo.finished).length > 0) {
    		progressText.value = "It's all systems go";
    	}
    }, {deep:true, immediate:true});

    defineExpose({ closeWrapper });
</script>



<template>
    <div style="background-color:rgb(39, 39, 39); border-radius:20px; padding:15px; margin-bottom:15px">

        <div v-on:click="toggleWrapper" style="display: flex; justify-content: space-between; align-items: center; cursor: pointer;">
            <div><slot name="header"></slot></div>
            <div :id="'arrow'+props.indexTodo" style="transform:rotate(180deg); transition-duration:300ms"><i class="bi bi-caret-up-fill" style="font-size:30px"></i></div>
        </div>

        
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

                <p style="margin-top:10px;">{{ progressText }}</p>
				<!-- <p v-if="finishedAll && todo.length > 0">All tasks done!</p> -->

                <div v-show="props.warningState" style="color:rgb(255, 62, 62); display:flex; flex-direction: column; align-items: center; margin-top:10px;">
		        	<p>Instead of adding new tasks, do existing ones!</p>
		        	<button v-on:click="hideWarning(indexTodo)">OK</button>
		        </div>

            </div>
        </div>

    </div>
</template>