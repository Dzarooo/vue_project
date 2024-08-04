<script setup>
    const props = defineProps({
        todos: {
            type: Array,
            required: false
        }
    });
</script>

<template>
    <div class="p-7 w-[20vw] min-w-[300px] shadow-lg rounded-xl [box-shadow:_0px_4px_24px_-4px_rgba(7,11,18,1);]"> <!-- todoList container -->
        <h2 class="text-2xl"><slot></slot></h2> <!-- title of todoList -->

        <span class="bg-slate-300 block w-full h-[1px] my-4"></span>

        <div v-for="todo in props.todos"> <!-- iterate through todo items and display them -->
            <p>{{ todo.title }}</p>
        </div>

    </div>
</template>