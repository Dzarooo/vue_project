<script setup>
    const model = defineModel();

    const props = defineProps({
        task: {
            type: String,
            required: true,
        },
        indexTodo: {
            type: Number,
            required: true,
        },
        indexItem: {
            type: Number,
            required: true,
        }
    });
    const emits = defineEmits([
        'changed'
    ]);

    function handleCheckboxToggle(indexItem, indexTodo) {
        emits('changed', indexItem, indexTodo);
    }
</script>

<template>
    <li v-if="model==false" style="display:flex; gap:10px;">
        <input type="button" value="Ukończ" :id="'task'+props.indexItem" v-on:click="handleCheckboxToggle(props.indexItem, props.indexTodo)">
        <label :for="'task'+props.indexItem">{{ props.task }}</label>
    </li>
    <li v-else style="display:flex; gap:10px;">
        <input type="button" value="Odznacz" :id="'task'+props.indexItem" v-on:click="handleCheckboxToggle(props.indexItem, props.indexTodo)">
        <label :for="'task'+props.indexItem" style="color:rgb(89, 89, 89); text-decoration-line: line-through">{{ props.task }}</label>
    </li>
</template>