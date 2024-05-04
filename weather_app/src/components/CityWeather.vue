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

        console.log("Prop searchResult changed in CityWeather.vue:", newValue);

        fetchData();
    });

    const todayWeatherData = ref({});
    const futureWeatherData = ref({
        one: {},
        two: {},
        three: {},
        four: {},
        five: {},
    });
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

    const isLoading = ref(false);

    function setIcon(icon) {
        //console.log(icon);
        return iconMap[icon];
    }

    function fetchData() {
        if(props.searchQuery !== "") {
            isLoading.value = true;
            //fetch today weather
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
                            const daysOfWeek = ['Niedziela','Poniedziałek','Wtorek','Środa','Czwartek','Piątek','Sobota'];
                            const dayOfWeek = daysOfWeek[a.getDay()];
                            const date = dayOfWeek+ ", " + day + " " + month + " " + year; 

                            const b = new Date(data.sys.sunrise * 1000);
                            const hourB = b.getHours();
                            const minutesB = b.getMinutes();
                            const sunrise = hourB + ":" + minutesB;

                            const c = new Date(data.sys.sunset * 1000);
                            const hourC = c.getHours();
                            const minutesC = c.getMinutes();
                            const sunset = hourC + ":" + minutesC;

                            todayWeatherData.value = {
                                name: data.name,
                                temp: data.main.temp.toFixed(1),
                                temp_min: data.main.temp_min.toFixed(1),
                                temp_max: data.main.temp_max.toFixed(1),
                                date: date,
                                day: day,
                                description: data.weather[0].description,
                                sunrise: sunrise,
                                sunset: sunset,
                                wind: data.wind.speed.toFixed(1),
                                icon: setIcon(data.weather[0].icon),
                            }
                            console.log(todayWeatherData.value)
                            searched.value = true;
                        }
	        	})
                .then(() => {
                    fetchFutureWeatherData();
                })
	        	.catch((error) => {
	        		console.error('Error fetching today weather data:\n', error.message);
	        	})
	        	.finally(() => {
	        	});
        } else {
            console.warn("searchQuery is empty in CityWeather.vue");
        }
    }

    function fetchFutureWeatherData() {
            //fetch weather for future days
            fetch(
                `https://api.openweathermap.org/data/2.5/forecast?q=${currentSearchQuery.value}&appid=f679cb1f60918bd9e72eece1168b0c17&units=metric&lang=pl`
                )
                .then((response) => response.json())
                .then((data) => {
                        if(data.message) {
                            console.error(data.message);
                            throw new Error(data.message);
                        }
                        else {
                            //console.log(data);

                            //variables for loop
                            let day = null;
                            let objectIndex = 0; //to which index in futureWeatherData append data
                            let timestampsCounter = 0; //8 timestamps equals one day

                            //variables for data reading in loop
                            let tempDay = 0;
                            let tempNight = 0;
                            let wind = 0;
                            let precipitationProbability = 0;
                            let dateD;

                            for(let i = 0; i < 5; ++i) { //api returns data for 5 days, that's why i < 5
                                for(let j = 0; j < 8; ++j) { //api returns 8 timestamps for each day, that's why j < 8
                                    let date = new Date(data.list[(j + (i * 8))].dt * 1000);
                                    let dayDate = date.getDate();

                                    //set objectIndex
                                    if(dayDate != day && dayDate !== todayWeatherData.value.day) {
                                        day = dayDate;
                                        objectIndex++;
                                    }

                                    if(dayDate === todayWeatherData.value.day) { //skip today because we are displaying data for future days
                                        continue
                                    }
                                    else { //this else is "endpoint" - here is everything with reading and appending data

                                        if(timestampsCounter == 8) { //one day has 8 timestamps, in this "if" append all data to futureWeatherData
                                            //console.log("new day");
                                            if(objectIndex-1 == 5) {
                                                timestampsCounter = 0;
                                                tempDay = 0;
                                                tempNight = 0;
                                                continue;
                                            }

                                            //append data to futureWeatherData
                                            //console.log("objectIndex: ", objectIndex)
                                            switch (objectIndex-1) {
                                                case 1:
                                                    futureWeatherData.value.one = {
                                                        tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                                        wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                                        date: dateD,
                                                    }
                                                    break;

                                                case 2:
                                                    futureWeatherData.value.two = {
                                                        tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                                        wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                                        date: dateD,
                                                    }
                                                    break;

                                                case 3:
                                                    futureWeatherData.value.three = {
                                                        tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                                        wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                                        date: dateD,
                                                    }
                                                    break;

                                                case 4:
                                                    futureWeatherData.value.four = {
                                                        tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                                        wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                                        date: dateD,
                                                    }
                                                    break;

                                                case 5:
                                                    futureWeatherData.value.five = {
                                                        tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                                        wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                                        precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                                        date: dateD,
                                                    }
                                                    break;
                                            
                                                default:
                                                    break;
                                                }

                                            //restore loop variables to default on new day
                                            timestampsCounter = 0;
                                            tempDay = 0;
                                            tempNight = 0;
                                            wind = 0;
                                            precipitationProbability = 0;
                                        }

                                        timestampsCounter++;
                                        
                                        if(timestampsCounter > 3) { // 9a.m. and later
                                            if(timestampsCounter == 4) {
                                                const d = new Date(data.list[(j + (i * 8))].dt * 1000);
                                                const monthsD = ['Styczeń','Luty','Marzec','Kwiecień','Maj','Czerwiec','Lipiec','Sierpień','Wrzesień','Październik','Listopad','Grudzień'];
                                                const yearD = d.getFullYear();
                                                const monthD = monthsD[d.getMonth()];
                                                const dayD = d.getDate();
                                                const daysOfWeekD = ['Niedziela','Poniedziałek','Wtorek','Środa','Czwartek','Piątek','Sobota'];
                                                const dayOfWeekD = daysOfWeekD[d.getDay()];
                                                dateD = dayOfWeekD+ ", " + dayD + " " + monthD + " " + yearD;
                                            }
                                            if(timestampsCounter !== 8) {
                                                tempDay += data.list[(j + (i * 8))].main.temp;
                                                wind += data.list[(j + (i * 8))].wind.speed;
                                                if(precipitationProbability < data.list[(j + (i * 8))].pop) {
                                                    precipitationProbability = data.list[(j + (i * 8))].pop;
                                                }
                                            }
                                            //console.log(1, objectIndex, "day:", dayDate, (j + (i * 8)), data.list[(j + (i * 8))].dt_txt, data.list[(j + (i * 8))].main.temp, data.list[(j + (i * 8))].pop);
                                        }
                                        else { // before 9a.m.
                                            tempNight += data.list[(j + (i * 8))].main.temp;
                                            //console.log(2, objectIndex, "day:", dayDate, (j + (i * 8)), data.list[(j + (i * 8))].dt_txt, data.list[(j + (i * 8))].main.temp);
                                        }

                                    }
                                }
                            }
                            futureWeatherData.value.five = {
                                tempDay: (((tempDay.toFixed(1)*10) / 4) / 10).toFixed(1),
                                tempNight: (((tempNight.toFixed(1)*10) / 3) / 10).toFixed(1),
                                wind: (((wind.toFixed(1)*10) / 4) / 10).toFixed(1),
                                precipitationProbability: (precipitationProbability * 100).toFixed(0),
                                date: dateD,
                            }
                            console.log(futureWeatherData.value);
                            
                        }
                    }
                )
                .catch((error) => {
	        		console.error('Error fetching today weather data:\n', error.message);
                })
                .finally(isLoading.value = false);
    }

</script>

<template>
    <div class="w-full flex-1 h-full flex flex-col">
        <div class="w-full flex-1 flex flex-col">
            <div v-if="searched" class="flex-1 flex flex-col justify-center items-center">
                <div v-if="isLoading" class="loadingContainer">
                    <div class="dot"></div>
                    <div class="dot"></div>
                    <div class="dot"></div>
                </div>
                <div v-else class="w-full flex-1 flex flex-col justify-around gap-10">
                    <div class="text-[#FFFFF0] flex justify-between font-thin mx-20">

                        <div>
                            <h1 class="text-8xl">{{ todayWeatherData.name }}</h1>
                            <p class="text-4xl text-slate-300 mt-4">{{ todayWeatherData.date }}</p>
                            <div class="w-fit">
                                <img class="w-[400px] h-[400px] mt-10" :src="todayWeatherData.icon">
                                <p class="text-4xl text-slate-300 text-center">{{ todayWeatherData.description }}</p>
                            </div>
                        </div>

                        <div class="flex">
                            <div class="flex flex-col items-center">
                                <h1 class="text-[20rem] [line-height:1] flex items-start">{{ todayWeatherData.temp }}</h1>   
                                <p class="text-8xl text-slate-300">{{ todayWeatherData.temp_min }}&deg; / {{ todayWeatherData.temp_max }}&deg;</p>
                                <div class="w-full flex justify-center gap-10 mt-10">
                                    <p class="text-5xl font-thin"><i class="bi bi-sunrise"></i> {{ todayWeatherData.sunrise }}</p>
                                    <p class="text-5xl font-thin">{{ todayWeatherData.sunset }} <i class="bi bi-sunset"></i></p>
                                </div>
                                <div class="text-5xl flex gap-2 mt-3"><i class="bi bi-wind"></i><p>{{ todayWeatherData.wind }}m/s</p></div>
                            </div>
                            <p><span class="text-8xl">&deg;C</span></p>
                        </div>

                    </div>

                    <div class="text-[#FFFFF0] font-thin flex justify-between gap-5 overflow-x-auto mx-20">
                        <template v-for="item in futureWeatherData">
                            <div class="min-w-[300px] [aspect-ratio:3/2] border-solid border-2 futureDaysDivBorderGradient relative p-2 rounded-xl flex-1">
                                <div class="absolute flex w-[calc(100%-16px)] h-[calc(100%-16px)] flex-col justify-center items-center">
                                    <div class="text-6xl flex items-center gap-2"><i class="bi bi-sun text-3xl [transform:translateY(3px)]"></i><p>{{ item.tempDay }}&deg;</p></div>
                                    <div class="text-3xl flex gap-1 items-center text-slate-300"><i class="bi bi-moon text-sm [transform:translateY(3px)]"></i><p>{{ item.tempNight }}&deg;</p></div>
                                </div>
                                <div class="h-full w-full flex flex-col justify-between">
                                    <div>
                                        <p>{{ item.date }}</p>
                                    </div>
                                    <div class="flex justify-around gap-2">
                                        <div class="flex gap-1"><i class="bi bi-wind"></i><p>{{ item.wind }}m/s</p></div>
                                        <div class="flex gap-1"><i class="bi bi-cloud-drizzle"></i><p>{{ item.precipitationProbability }}%</p></div>
                                    </div>
                                </div>
                            </div>
                        </template>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>