<script setup>
    import { reactive, ref } from 'vue';

    const props = defineProps({
        username: {
            type: String,
            required: true
        },
    });

    const data = ref([]);

    function load() {
        fetch(props.username)
            .then(response => response.json())
            .then(result => {data.value[0] = result});
    }

    load();

</script>

<template>
    <div>
        <h1>{{ data[0].login }}</h1>
        <p>{{ data[0].name }}</p>
        <p>{{ data[0].bio }}</p>
        <p>{{ data[0].avatar_url }}</p>
        <p>{{ data[0].followers }}</p>
        <p>{{ data[0].location }}</p>
    </div>
</template>