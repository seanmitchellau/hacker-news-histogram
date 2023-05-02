<script setup>
import {ref} from 'vue';
import axios from 'axios';
import UserSearch from '~/components/UserSearch.vue';

const loading = ref(false);
const histogram = ref([]);

const searchUser = async (username) => {
    loading.value = true;

    try {
        // https://github.com/HackerNews/API
        // first find user posts/items
        const userResponse = await axios.get(`https://hacker-news.firebaseio.com/v0/user/${username}.json`);
        const user = userResponse.data;

        if (!user || !user.submitted) { // sanity check
            histogram.value = [];
            loading.value = false;
            return;
        }

        // hopefully 500 concurrent requests is not too much for the API or the browser
        const storiesPromises = user.submitted.slice(0, 500).map(id =>
            axios.get(`https://hacker-news.firebaseio.com/v0/item/${id}.json`),
        );
        const storiesResponses = await Promise.all(storiesPromises);

        const stories = storiesResponses.map(response => response.data);
        const histogramData = Array(24).fill(0);

        // populate histogram data array
        stories.forEach(story => {
            if (story && story.time) {
                const date = new Date(story.time * 1000);
                const hour = date.getHours();
                histogramData[hour]++;
            }
        });

        histogram.value = histogramData;
    } catch (error) {
        console.error('Error fetching data', error);
    } finally {
        loading.value = false;
    }
};
</script>

<template>
    <div>
        <UserSearch @search="searchUser" :loading="loading"/>

        <div class="histogram">
            <div v-if="loading">Retrieving data. Please wait...</div>

            <div v-else-if="histogram.length">
                {{ histogram }}
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.histogram {
  margin: 30px;
}
</style>
