<script setup>
    import GitHubCard from './GitHubCard.vue';

    const props = defineProps({
        users: {
            type: Array,
            required:true
        },
        searchUsers: {
            type: String,
            required:false
        }
    });

</script>


<template>
    <div class="flex gap-[50px] w-[100vw] flex-wrap justify-center">
        <template v-for="user in props.users">
            <GitHubCard v-bind:username="user"/>
        </template>
        <template v-if='searchUsers != ""'>
            <GitHubCard v-bind:username="searchUsers"></GitHubCard>
        </template>
    </div>
</template>