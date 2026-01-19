<script setup>
    import { ref } from 'vue';

    const activeUnit = ref("celsius");

    const emits = defineEmits([
        "unitChanged",
    ]);

    function unitChanged(e) {
        let newUnit = e.target.value;
        //console.log(newUnit);
        emits("unitChanged", newUnit);
    }
</script>

<template>
    <div class="h-[32px] w-fit bg-transparent border-solid border-[1px] border-slate-300 rounded-full shadow-md flex items-center"> <!-- container -->

        <div> <!-- first radio input -->
            <input type="radio" name="temperatureUnit" id="celsius" value="celsius" class="hidden" v-model="activeUnit" v-on:input="unitChanged">
            <label for="celsius" :class="{'bg-white text-slate-500': activeUnit === 'celsius'}" class="text-slate-200 cursor-pointer select-none px-2 mx-1 pb-[1px] rounded-xl transition-all duration-300">&deg;C</label>
        </div>

        <div> <!-- second radio input -->
            <input type="radio" name="temperatureUnit" id="fahrenheit" value="fahrenheit" class="hidden" v-model="activeUnit" v-on:input="unitChanged">
            <label for="fahrenheit" :class="{'bg-white text-slate-500': activeUnit === 'fahrenheit'}" class="text-slate-200 cursor-pointer select-none text-center rounded-xl pb-[1px] px-2 mx-1 transition-all duration-300">&deg;F</label>
        </div>

    </div>
</template>