<script setup>
    const props = defineProps({
        todos: {
            type: Array,
            required: false
        }
    });
</script>

<template>
    <h2 class="text-2xl"><slot></slot></h2>
    <div v-for="todo in props.todos">
        <p>{{ todo.title }}</p>
    </div>
</template>