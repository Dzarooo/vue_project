<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        completed: {
            type: Boolean,
            required: true,
        }
    })

    const completed = ref(props.completed);

    function toggleCheckbox() {
        if(completed.value) {
            completed.value = false;
        }
        else {
            completed.value = true;
        }
    };

</script>

<template>

    <div class="flex items-center gap-2"> <!-- todo container -->
        <div v-on:click="toggleCheckbox" :class="{ 'bg-slate-400':completed }" class="w-3 h-3 border-solid border-[2px] border-white rounded-xl hover:cursor-pointer"></div>
        <p :class="{ 'line-through text-slate-500 hover:!border-b-slate-500': completed }" class="border-transparent border-b-[1px] hover:border-b-white"><slot></slot></p>
    </div>

</template>