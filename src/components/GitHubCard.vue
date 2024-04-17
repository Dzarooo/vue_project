<script setup>
    import { reactive, ref, toRaw } from 'vue';

    const props = defineProps({
        username: {
            type: String,
            required: true
        },
    });

    const data = ref({});

    async function load() {
        try {
            const response = await fetch("https://api.github.com/users/"+props.username);
            if (!response.ok) {
                data.value = null;
                throw new Error('Failed to fetch user data');
            }
            const result = await response.json();
            data.value = result;
        } catch (error) {
            console.error("An error occurred:", error);
            // You can handle the error here, such as displaying a message to the user or logging it
        }
    }

    //async load() {
    //    const response = await fetch("https://api.github.com/users/"+props.username);
    //    const result = await response.json()
    //    data.value = result
    //}


    load();
    console.log(data.value);


</script>

<template>
    <div class="min-w-[300px] w-[300px] min-h-[400px] p-10 shadow-lg bg-white rounded-sm">
        <template v-if="data != null">
            <div class="w-full flex flex-col items-center mb-8">
                <img class="w-[100px] h-[100px]" src="https://static.vecteezy.com/system/resources/thumbnails/019/896/012/small_2x/female-user-avatar-icon-in-flat-design-style-person-signs-illustration-png.png">
                <h1 class="text-2xl">{{ data.login }}</h1>
                <p class="text-sm">{{ data.name }}</p>
            </div>
            <p class="text-sm mb-8">{{ data.bio }}</p>
            
        </template>

        <template v-else>
            <h1 class="text-center">Couldn't retrieve data about user {{props.username}}</h1>
        </template>
    </div>
</template>