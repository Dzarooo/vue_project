<script setup>
    import { watch } from 'vue';

    const props = defineProps({
        searchQuery: {
            type: String,
            required: true,
        },
    })

    console.log("data received in CityWeather.vue")
    console.log(props.searchQuery);

    // if(props.searchQuery !== "") {
    // fetch(
	// 	`https://api.openweathermap.org/data/2.5/weather?q=${props.searchQuery}&appid=f679cb1f60918bd9e72eece1168b0c17&lang=pl`
	//     )
	// 	.then((response) => response.json())
	// 	.then((data) => {
    //             if(data.message) {
    //                 console.error(data.message);
    //                 throw new Error(data.message);
    //             }
    //             else {
    //                 weatherData.value = data;
    //                 emits('searched', weatherData.value);
    //               //console.log(weatherData.value)
    //             }
	// 	})
	// 	.catch((error) => {
	// 		console.error('Error fetching weather data:\n', error.message);
	// 	})
	// 	.finally(() => {
	// 	});
    // }
    // else {
    //     console.warn("searchQuery is empty in CityWeather.vue");
    // }

</script>

<template>
    <div class="w-full flex-1">
        <div class="text-[#FFFFF0]">
            <h1 class="text-[20rem] [line-height:1] flex items-start">31<span class="text-8xl">°C</span></h1>
            <p v-if="searchResult">Prop: {{ props.searchResult.td }}</p>
        </div>
    </div>
</template>