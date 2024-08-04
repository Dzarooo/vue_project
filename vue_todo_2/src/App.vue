<script setup>
import todoList from "./components/todoList.vue";
import { reactive } from "vue";

const todoLists = reactive({
    1: {
        title: "List 1",
        todos: [
            { id: 1, title: "Task 1", completed: false },
            { id: 2, title: "Task 2", completed: true }
        ]
    },
    2: {
        title: "List 2",
        todos: [
            { id: 3, title: "Task 3", completed: false },
            { id: 4, title: "Task 4", completed: true }
        ]
    },
    3: {
        title: "List 3",
        todos: [
            { id: 5, title: "Task 5", completed: false },
            { id: 6, title: "Task 6", completed: true }
        ]
    },
    4: {
        title: "List 4",
        todos: [
            { id: 7, title: "Task 7", completed: false },
            { id: 8, title: "Task 8", completed: true }
        ]
    },
    5: {
        title: "List 5",
        todos: [
            { id: 9, title: "Task 9", completed: false },
            { id: 10, title: "Task 10", completed: true }
        ]
    }
})
</script>

<template>

    <div class="w-full min-h-[100vh] bg-slate-900 text-slate-200"> <!-- default div, 100% space of display, background color and white text in whole website -->

        <div class="pt-10 flex items-center justify-center gap-5 w-full"> <!-- header -->
            <span class="bg-white w-20 h-[2px] translate-y-[3px]"></span>
            <h1 class="text-5xl">Todo Lists</h1>
            <span class="bg-white w-20 h-[2px] translate-y-[3px]"></span>
        </div>

        <div class="mt-10 flex flex-wrap justify-center gap-10">
            <template v-for="list in todoLists"> <!-- iterate through every todolist and display component for each of them -->
                <todoList v-bind:todos="list.todos">{{ list.title }}</todoList>
            </template>
        </div>
    </div>
</template>