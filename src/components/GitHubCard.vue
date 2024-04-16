<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        username: {
            type: String,
            required: true
        },
    });

    const data = ref([{
        "login": "MattMoszczynski",
        "id": 10995478,
        "node_id": "MDQ6VXNlcjEwOTk1NDc4",
        "avatar_url": "https://avatars.githubusercontent.com/u/10995478?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/MattMoszczynski",
        "html_url": "https://github.com/MattMoszczynski",
        "followers_url": "https://api.github.com/users/MattMoszczynski/followers",
        "following_url": "https://api.github.com/users/MattMoszczynski/following{/other_user}",
        "gists_url": "https://api.github.com/users/MattMoszczynski/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/MattMoszczynski/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/MattMoszczynski/subscriptions",
        "organizations_url": "https://api.github.com/users/MattMoszczynski/orgs",
        "repos_url": "https://api.github.com/users/MattMoszczynski/repos",
        "events_url": "https://api.github.com/users/MattMoszczynski/events{/privacy}",
        "received_events_url": "https://api.github.com/users/MattMoszczynski/received_events",
        "type": "User",
        "site_admin": false,
        "name": "Mateusz Moszczyński",
        "company": null,
        "blog": "",
        "location": "Szczecin, Poland",
        "email": null,
        "hireable": null,
        "bio": "An average keyboard enjoyer ⌨️",
        "twitter_username": null,
        "public_repos": 8,
        "public_gists": 0,
        "followers": 10,
        "following": 6,
        "created_at": "2015-02-13T16:30:29Z",
        "updated_at": "2024-03-01T14:15:19Z"
    }]);

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