<script setup>
    import { ref, watch } from 'vue';

    //props
    const props = defineProps({
        searchQuery: {
            type: String,
            required: true,
        },
        temperatureUnit: {
            type: String,
            required: false,
        }
    });

    //emits
    const emits = defineEmits([
        'backgroundChanged',
    ])

    //searchQuery
    const currentSearchQuery = ref(props.searchQuery);

    watch(() => props.searchQuery, (newValue, oldValue) => {
        currentSearchQuery.value = newValue;

        console.log("Prop searchQuery changed in CityWeather.vue:", newValue);

        fetchData();
    });

    //temperatureUnit
    const currentTemperatureUnit = ref("celsius");

    watch(() => props.temperatureUnit, (newValue, oldValue) => {
        currentTemperatureUnit.value = newValue;

        console.log("Prop temperatureUnit changed in CityWeather.vue:", currentTemperatureUnit.value);
        todayWeatherData.value.unit = newValue;
        console.log(todayWeatherData.value)
        console.log(futureWeatherData.value)
    })

    //weatherData variables
    const todayWeatherData = ref({});
    const futureWeatherData = ref({
        one: {},
        two: {},
        three: {},
        four: {},
        five: {},
    });
    //variables for idiot-protection
    const searched = ref(false);
    const isLoading = ref(false);
    const isCityFound = ref(false);

    //map for setting weather icon according to data from fetch
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

    //set weather icon according to data from fetch
    function setIcon(icon) {
        //console.log(icon);
        return iconMap[icon];
    }

    //map for setting background image according to data from fetch
    const backgroundMap = {
		'01d': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://images.pexels.com/photos/96622/pexels-photo-96622.jpeg?cs=srgb&dl=pexels-wdnet-96622.jpg&fm=jpg')",
		'01n': "linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)), url('https://static.vecteezy.com/system/resources/previews/035/987/337/large_2x/ai-generated-clear-night-sky-filled-with-stars-that-seem-to-twinkle-against-a-dark-backdrop-free-photo.jpg')",
		'02d': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://images.unsplash.com/photo-1632117761438-b8ce7fc6838e?q=80&w=2670&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D')",
		'02n': "linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)), url('https://www.fireworks-videos.com/wp-content/uploads/2021/11/cloudy-night-sky-with-moon-and-stars.jpg')",
		'03d': "linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), url('https://images.pexels.com/photos/158163/clouds-cloudporn-weather-lookup-158163.jpeg?cs=srgb&dl=pexels-pixabay-158163.jpg&fm=jpg')",
		'03n': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://wallpapers.com/images/hd/dark-cloudy-night-sky-moon-rj9llmsuas28ih98.jpg')",
		'04d': "linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), url('https://i.pinimg.com/originals/c5/ae/10/c5ae102d1e5e2f80a3d39cf7beda1772.jpg')",
		'04n': "linear-gradient(rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.8)), url('https://i.pinimg.com/originals/40/78/c5/4078c55eb332735d1bcd060f4bf4b715.jpg')",
		'09d': "linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), url('https://images.pexels.com/photos/4997340/pexels-photo-4997340.jpeg?cs=srgb&dl=pexels-chris-f-38966-4997340.jpg&fm=jpg')",
		'09n': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://i3.wp.com/static.vecteezy.com/system/resources/thumbnails/011/406/662/original/night-rain-4k-loop-rain-drops-falling-in-rainy-season-free-video.jpg?ssl=1')",
		'10d': "linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://static.vecteezy.com/system/resources/thumbnails/039/159/827/small_2x/raining-dark-cloudy-storm-cloud-nature-in-rainy-season-photo.jpg')",
		'10n': "linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://c.wallhere.com/photos/5a/77/rain_nature-122884.jpg!d')",
		'11d': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://www.microstep-mis.com/drupal/web/sites/default/files/2021-12/Lightning-and-thunderstorm.jpg')",
		'11n': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://c02.purpledshub.com/uploads/sites/41/2020/08/GettyImages-NA006117-b5eac24.jpg')",
		'13d': "linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://bucket-otsnews.s3.us-west-2.amazonaws.com/wp-content/uploads/sites/13/2021/09/Weather_snowing_mountain_wind_resized_.jpg')",
		'13n': "linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://fournews-assets-prod-s3b-ew1-aws-c4-pml.s3.amazonaws.com/media/2017/12/snow_london_g_hd.jpg')",
		'50d': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://www.metoffice.gov.uk/binaries/content/gallery/metofficegovuk/hero-images/weather/fog--mist/foggy-morning-in-a-meadow.jpg')",
		'50n': "linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url('https://c.pxhere.com/photos/41/db/fog_light_beams_natural_bright_foggy_misty_magic-915901.jpg!d')",
    }

    //set background image according to data from fetch
    function setBackground(icon) {
        emits("backgroundChanged", backgroundMap[icon]);
    }

    //fetch data
    function fetchData() {
        if(props.searchQuery !== "") {
            isLoading.value = true;
            searched.value = true;
            isCityFound.value = true;
            let tempIconIndex;
            //fetch today weather
            fetch(
	        	`https://api.openweathermap.org/data/2.5/weather?q=${currentSearchQuery.value}&appid=f679cb1f60918bd9e72eece1168b0c17&units=metric&lang=pl`
	            )
	        	.then((response) => response.json())
	        	.then((data) => {
                        if(data.message) {
                            console.error(data.message);
                            isCityFound.value = false;
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
                            const minutesB = b.getMinutes() >= 10 ? b.getMinutes() : "0" + b.getMinutes();
                            const sunrise = hourB + ":" + minutesB;

                            const c = new Date(data.sys.sunset * 1000);
                            const hourC = c.getHours();
                            const minutesC = c.getMinutes() >= 10 ? c.getMinutes() : "0" + c.getMinutes();
                            const sunset = hourC + ":" + minutesC;

                            todayWeatherData.value = {
                                name: data.name,
                                temp: data.main.temp.toFixed(1),
                                unit: currentTemperatureUnit.value == "celsius" ? "celsius" : "fahrenheit",
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
                            isCityFound.value = true;
                            tempIconIndex = data.weather[0].icon;
                        }
	        	})
                .then(() => {
                    fetchFutureWeatherData(tempIconIndex);
                })
	        	.catch((error) => {
	        		console.error('Error fetching today weather data:\n', error.message);
                    emits("backgroundChanged", "");
	        	})
        } else {
            console.warn("searchQuery is empty in CityWeather.vue");
        }
    }

    function fetchFutureWeatherData(iconIndex) { //iconIndex is used to set background of website based on weather in searched city
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
	        		console.error('Error fetching future weather data:\n', error.message);
                })
                .finally(() => {
                    isLoading.value = false;
                    setBackground(iconIndex)
                });
    }

</script>

<template>
    <div class="w-full flex-1 h-full flex flex-col">
        <div class="w-full flex-1 flex flex-col">
            <div v-if="searched" class="flex-1 flex flex-col justify-center items-center">
                <div v-if="!isCityFound" class="text-slate-300 flex flex-col items-center justify-center">
                    <i class="bi bi-eye-slash text-8xl"></i>
                    <p class="text-3xl">City not found.</p>
                </div>
                <div v-else-if="isLoading" class="loadingContainer">
                    <div class="dot"></div>
                    <div class="dot"></div>
                    <div class="dot"></div>
                </div>
                <div v-else class="w-full flex-1 flex flex-col justify-around gap-10">
                    <div class="text-[#FFFFF0] flex justify-between font-thin mx-20">

                        <div>
                            <h1 class="text-[3rem] lg:text-[4.5rem] xl:text-8xl">{{ todayWeatherData.name }}</h1>
                            <p class="text-[1.25rem] lg:text-[1.87rem] xl:text-4xl text-slate-300 mt-[0.5rem] lg:mt-[0.75rem] xl:mt-4">{{ todayWeatherData.date }}</p>
                            <div class="w-fit">
                                <img class="w-[200px] lg:w-[300px] xl:w-[400px] h-[200px] lg:h-[300px] xl:h-[400px] mt-[1.25rem] lg:mt-[1.87rem] xl:mt-10" :src="todayWeatherData.icon">
                                <p class="text-[1.25rem] lg:text-[1.87rem] xl:text-4xl text-slate-300 text-center">{{ todayWeatherData.description }}</p>
                            </div>
                        </div>

                        <div class="flex">
                            <div class="flex flex-col items-center">
                                <template v-if="todayWeatherData.unit == 'celsius'">
                                    <h1 class="text-[10rem] lg:text-[15rem] xl:text-[20rem] [line-height:1] flex items-start">{{ todayWeatherData.temp }}</h1>   
                                    <p class="text-[3rem] lg:text-[4.5rem] xl:text-8xl text-slate-300">{{ todayWeatherData.temp_min }}&deg; / {{ todayWeatherData.temp_max }}&deg;</p>
                                </template>
                                <template v-else>
                                    <h1 class="text-[10rem] lg:text-[15rem] xl:text-[20rem] [line-height:1] flex items-start">{{ ((todayWeatherData.temp * 1.8) + 32).toFixed(1) }}</h1>   
                                    <p class="text-[3rem] lg:text-[4.5rem] xl:text-8xl text-slate-300">{{ ((todayWeatherData.temp_min * 1.8) + 32).toFixed(1) }}&deg; / {{ ((todayWeatherData.temp_max * 1.8) + 32).toFixed(1) }}&deg;</p>
                                </template>
                                <div class="w-full flex justify-center gap-10 mt-[1.25rem] lg:mt-[1.87rem] xl:mt-10">
                                    <p class="text-[1.5rem] lg:text-[2.25rem] xl:text-5xl font-thin"><i class="bi bi-sunrise"></i> {{ todayWeatherData.sunrise }}</p>
                                    <p class="text-[1.5rem] lg:text-[2.25rem] xl:text-5xl font-thin">{{ todayWeatherData.sunset }} <i class="bi bi-sunset"></i></p>
                                </div>
                                <div class="text-5xl flex gap-2 mt-[0.37rem] lg:mt-[0.56rem] xl:mt-3"><i class="bi bi-wind"></i><p>{{ todayWeatherData.wind }}m/s</p></div>
                            </div>
                            <template v-if="todayWeatherData.unit == 'celsius'">
                                <p><span class="text-8xl">&deg;C</span></p>
                            </template>
                            <template v-else>
                                <p><span class="text-8xl">&deg;F</span></p>
                            </template>
                        </div>

                    </div>

                    <div class="text-[#FFFFF0] font-thin flex justify-between gap-5 overflow-x-auto futureDaysScrollbar pb-4 mb-5 mx-20">
                        <template v-for="item in futureWeatherData">
                            <div class="min-w-[300px] [aspect-ratio:3/2] border-solid border-2 futureDaysDivBorderGradient relative p-2 rounded-xl flex-1">
                                <div class="absolute flex w-[calc(100%-16px)] h-[calc(100%-16px)] flex-col justify-center items-center">
                                    <template v-if="todayWeatherData.unit == 'celsius'">
                                        <div class="text-6xl flex items-center gap-2"><i class="bi bi-sun text-3xl [transform:translateY(3px)]"></i><p>{{ item.tempDay }}&deg;</p></div>
                                        <div class="text-3xl flex gap-1 items-center text-slate-300"><i class="bi bi-moon text-sm [transform:translateY(3px)]"></i><p>{{ item.tempNight }}&deg;</p></div>
                                    </template>
                                    <template v-else>
                                        <div class="text-6xl flex items-center gap-2"><i class="bi bi-sun text-3xl [transform:translateY(3px)]"></i><p>{{ ((item.tempDay * 1.8) + 32).toFixed(1) }}&deg;</p></div>
                                        <div class="text-3xl flex gap-1 items-center text-slate-300"><i class="bi bi-moon text-sm [transform:translateY(3px)]"></i><p>{{ ((item.tempNight * 1.8) + 32).toFixed(1) }}&deg;</p></div>
                                    </template>
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