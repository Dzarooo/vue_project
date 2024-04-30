<script setup>
    import { ref } from 'vue';

    const searchInput = ref("");
    const weatherData = ref({});

    const emits = defineEmits([
        'searched'
    ]);

    

    function search() {
        console.log(searchInput.value);
        if(searchInput.value != "") {
            fetch(
				`https://api.openweathermap.org/data/2.5/weather?q=${searchInput.value}&appid=f679cb1f60918bd9e72eece1168b0c17&lang=pl`
			    )
				.then((response) => response.json())
				.then((data) => {
                    if(data.message) {
                        throw new Error(data.message);
                        console.error(data.message);
                    }
                    else {
                        weatherData.value = data;
                        emits('searched', weatherData.value);
                        //console.log(weatherData.value)
                    }
				})
				.catch((error) => {
					console.error('Error fetching weather data:\n', error.message);
				})
				.finally(() => {
				});
        }
    }

</script>

<template>
    <div class="w-fit h-fit flex gap-2 bg-[rgba(255,255,255,0.5)] border-solid border-[1px] border-slate-500 rounded-full items-center justify-around px-2 shadow-md">
        <input v-model="searchInput" class="w-[400px] appearance-none rounded-full p-1 outline-none text-slate-500" placeholder="Search for city...">
        <div class="[transform:translateY(-7%)] select-none">
            <p class="text-slate-500">|</p>
        </div>
        <div v-on:click="search()" class="cursor-pointer">
            <i class="bi bi-search text-slate-500 hover:text-slate-900"></i>
        </div>
    </div>
</template>