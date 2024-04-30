<script setup>
    import { ref, watch } from 'vue';

    const props = defineProps({
        searchQuery: {
            type: String,
            required: true,
        },
    });

    const currentSearchQuery = ref(props.searchQuery);

    watch(() => props.searchQuery, (newValue, oldValue) => {
        currentSearchQuery.value = newValue;

        console.log("Prop searchResult changed in CityWeather.vue");
        console.log("New value:", newValue);

        fetchData();
    });


    function fetchData() {
        if(props.searchQuery !== "") {
            fetch(
	        	`https://api.openweathermap.org/data/2.5/weather?q=${currentSearchQuery.value}&appid=f679cb1f60918bd9e72eece1168b0c17&lang=pl`
	            )
	        	.then((response) => response.json())
	        	.then((data) => {
                        if(data.message) {
                            console.error(data.message);
                            throw new Error(data.message);
                        }
                        else {
                            weatherData.value = data;
                          //console.log(weatherData.value)
                        }
	        	})
	        	.catch((error) => {
	        		console.error('Error fetching weather data:\n', error.message);
	        	})
	        	.finally(() => {
	        	});
            }
        else {
            console.warn("searchQuery is empty in CityWeather.vue");
        }
    }

</script>

<template>
    <div class="w-full flex-1">
        <div class="text-[#FFFFF0]">
            <h1 class="text-[20rem] [line-height:1] flex items-start">31<span class="text-8xl">&deg;C</span></h1>
        </div>
    </div>
</template>