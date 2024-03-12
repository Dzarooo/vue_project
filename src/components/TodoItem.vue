<script setup>
    const props = defineProps({
        task: {
            type: String,
            required: true,
        },
        completed: {
            type: Boolean,
            default: false,
        },
        index: {
            type: Number,
            required: true,
        }
    });
    const emits = defineEmits([
        'changed'
    ]);

    function handleCheckboxToggle(e, index) {
        let state = e.target.checked;
        emits('changed', state, index);
    }
</script>

<template>
    <li>
        <input type="checkbox" v-bind:checked="props.completed" v-on:change="handleCheckboxToggle($event, props.index)">
        {{ props.task }}
    </li>
</template>