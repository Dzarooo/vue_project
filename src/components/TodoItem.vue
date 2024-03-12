<script setup>
    const model = defineModel();

    const props = defineProps({
        task: {
            type: String,
            required: true,
        },
        index: {
            type: Number,
            required: true,
        }
    });
    const emits = defineEmits([
        'changed'
    ]);

    function handleCheckboxToggle(index) {
        emits('changed', index);
    }
</script>

<template>
    <li v-if="model==false" style="display:flex; gap:10px;">
        <input type="button" value="Ukończ" :id="'task'+props.index" v-on:click="handleCheckboxToggle(props.index)">
        <label :for="'task'+props.index">{{ props.task }}</label>
    </li>
    <li v-else style="display:flex; gap:10px;">
        <input type="button" value="Odznacz" :id="'task'+props.index" v-on:click="handleCheckboxToggle(props.index)">
        <label :for="'task'+props.index" style="color:rgb(89, 89, 89); text-decoration-line: line-through">{{ props.task }}</label>
    </li>
</template>