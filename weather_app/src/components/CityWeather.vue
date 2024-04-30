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

    const weatherData = ref({});
    const searched = ref(false);

    const iconMap = {
		'01d': 'https://d.iplsc.com/weather/svg-icons/1.svg',
		'01n': 'https://d.iplsc.com/weather/svg-icons/33.svg',
		'02d': 'https://d.iplsc.com/weather/svg-icons/3.svg',
		'02n': 'https://d.iplsc.com/weather/svg-icons/35.svg',
		'03d': 'https://d.iplsc.com/weather/svg-icons/7.svg',
		'03n': 'https://d.iplsc.com/weather/svg-icons/7.svg',
		'04d': 'https://d.iplsc.com/weather/svg-icons/8.svg',
		'04n': 'https://d.iplsc.com/weather/svg-icons/8.svg',
		'09d': 'https://d.iplsc.com/weather/svg-icons/18.svg',
		'09n': 'https://d.iplsc.com/weather/svg-icons/18.svg',
		'10d': 'https://d.iplsc.com/weather/svg-icons/13.svg',
		'10n': 'https://d.iplsc.com/weather/svg-icons/40.svg',
		'11d': 'https://d.iplsc.com/weather/svg-icons/15.svg',
		'11n': 'https://d.iplsc.com/weather/svg-icons/15.svg',
		'13d': 'https://d.iplsc.com/weather/svg-icons/19.svg',
		'13n': 'https://d.iplsc.com/weather/svg-icons/19.svg',
		'50d': 'https://d.iplsc.com/weather/svg-icons/5.svg',
		'50n': 'https://d.iplsc.com/weather/svg-icons/37.svg',
	};

    function setIcon(icon) {
        console.log(icon);
        return iconMap[icon];
    }

    function fetchData() {
        if(props.searchQuery !== "") {
            fetch(
	        	`https://api.openweathermap.org/data/2.5/weather?q=${currentSearchQuery.value}&appid=f679cb1f60918bd9e72eece1168b0c17&units=metric&lang=pl`
	            )
	        	.then((response) => response.json())
	        	.then((data) => {
                        if(data.message) {
                            console.error(data.message);
                            throw new Error(data.message);
                        }
                        else {
                            const a = new Date(data.dt * 1000);
                            const months = ['Styczeń','Luty','Marzec','Kwiecień','Maj','Czerwiec','Lipiec','Sierpień','Wrzesień','Październik','Listopad','Grudzień'];
                            const year = a.getFullYear();
                            const month = months[a.getMonth()];
                            const day = a.getDate();
                            const daysOfWeek = ['Poniedziałek','Wtorek','Środa','Czwartek','Piątek','Sobota','Niedziela'];
                            const dayOfWeek = daysOfWeek[a.getDay()-1];
                            const date = dayOfWeek+ ", " + day + " " + month + " " + year; 

                            const b = new Date(data.sys.sunrise * 1000);
                            const hourB = b.getHours();
                            const minutesB = b.getMinutes();
                            const sunrise = hourB + ":" + minutesB;

                            const c = new Date(data.sys.sunset * 1000);
                            const hourC = c.getHours();
                            const minutesC = c.getMinutes();
                            const sunset = hourC + ":" + minutesC;

                            weatherData.value = {
                                name: data.name,
                                temp: data.main.temp.toFixed(1),
                                temp_min: data.main.temp_min.toFixed(1),
                                temp_max: data.main.temp_max.toFixed(1),
                                date: date,
                                description: data.weather[0].description,
                                sunrise: sunrise,
                                sunset: sunset,
                                wind: data.wind.speed,
                                icon: setIcon(data.weather[0].icon),
                            }
                            console.log(weatherData.value)
                            searched.value = true;
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

    fetchData();

</script>

<template>
    <div class="w-full flex-1">
        <div class="w-[calc(100%-10rem)] mt-40 mx-20">
            <div v-if="searched">
                <div class="text-[#FFFFF0] flex justify-between font-thin">

                    <div>
                        <h1 class="text-8xl">{{ weatherData.name }}</h1>
                        <p class="text-4xl text-slate-300 mt-4">{{ weatherData.date }}</p>
                        <div class="w-fit">
                            <img class="w-[400px] h-[400px] mt-10" :src="weatherData.icon">
                            <p class="text-4xl text-slate-300 text-center">{{ weatherData.description }}</p>
                        </div>
                    </div>

                    <div class="flex">
                        <div class="flex flex-col items-center">
                            <h1 class="text-[20rem] [line-height:1] flex items-start">{{ weatherData.temp }}</h1>   
                            <p class="text-8xl text-slate-300">{{ weatherData.temp_min }}&deg; / {{ weatherData.temp_max }}&deg;</p>
                            <div class="w-full flex justify-center gap-10 mt-10">
                                <p class="text-5xl font-thin"><i class="bi bi-sunrise"></i> {{ weatherData.sunrise }}</p>
                                <p class="text-5xl font-thin">{{ weatherData.sunset }} <i class="bi bi-sunset"></i></p>
                            </div>
                            <div class="text-5xl flex gap-2 mt-3"><i class="bi bi-wind"></i><p>{{ weatherData.wind }}km</p></div>
                        </div>
                        <p><span class="text-8xl">&deg;C</span></p>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>