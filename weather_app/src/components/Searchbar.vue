<script setup>
    import { ref } from 'vue';

    const searchInput = ref("");

    const emits = defineEmits([
        'searched'
    ]);

    

    function search() {
        if(searchInput.value != "") {
            emits('searched', searchInput.value);
        }
    }

</script>

<template>
    <div class="w-fit h-[32px] flex gap-2 bg-transparent border-solid border-[1px] border-slate-300 rounded-full items-center justify-around px-2 shadow-md">
        <input v-model="searchInput" v-on:keyup.enter="search()" class="w-[400px] appearance-none rounded-full p-1 outline-none text-slate-200 bg-transparent placeholder:text-slate-200" placeholder="Search for city...">
        <div class="[transform:translateY(-7%)] select-none">
            <p class="text-slate-200">|</p>
        </div>
        <div v-on:click="search()" class="cursor-pointer">
            <i class="bi bi-search text-slate-200 hover:text-slate-100"></i>
        </div>
    </div>
</template>