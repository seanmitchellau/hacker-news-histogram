<script setup>
import {ref} from 'vue';
import axios from 'axios';
import UserSearch from '~/components/UserSearch.vue';
import Chart from "~/components/Chart.vue";

const loading = ref(false);
const histogram = ref([]);
const resolvedPromises = ref(0);
const totalPromises = ref(0);

const searchUser = async (username) => {
    loading.value = true;
    resolvedPromises.value = 0;
    totalPromises.value = 0;

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
        const storiesPromises = user.submitted.slice(0, 500).map((id) => {
            totalPromises.value++;
            return axios
                .get(`https://hacker-news.firebaseio.com/v0/item/${id}.json`)
                .then((response) => {
                    resolvedPromises.value++;
                    return response;
                });
        });
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
            <div v-if="loading">
                Retrieving data. Please wait...
                <div class="loading-bar">
                    <div class="progress" :style="{ width: (resolvedPromises / totalPromises * 100) + '%' }"></div>
                </div>
                <small>({{ resolvedPromises }} / {{ totalPromises }})</small>
            </div>

            <div v-else-if="histogram.length">
                <Chart :chart-data="histogram"/>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.histogram {
  margin: 30px;

    .loading-bar {
        margin: 20px auto;
        max-width: 300px;
        padding: 3px;
        background: #ccc;
        border-radius: 5px;

        .progress {
            background: green;
            height: 3px;
            border-radius: 5px;
        }
    }
}
</style>
