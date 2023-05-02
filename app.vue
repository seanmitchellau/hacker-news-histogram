<script setup>
import Histogram from "~/components/Histogram.vue";

</script>

<template>
    <div>
        <div class="container">
            <div>
                <h1>Hacker News - Histogram</h1>
            </div>

            <Histogram/>
        </div>

        <small>
            <a href="https://github.com/seanmitchellau/hacker-news-histogram" target="_blank">GitHub</a>
        </small>
    </div>
</template>

<style lang="scss">
body {
  font-family: Arial, serif;
  background: rgb(174, 222, 238);
  background: radial-gradient(circle, rgba(174, 222, 238, 0.7) 40%, rgba(148, 175, 233, 0.7) 100%);
  text-align: center;


  .container {
    margin: 5rem auto;
    width: 80%;
    max-width: 1100px;
    padding: 30px;
    background: #fafafa;
    border-radius: 20px;
    box-shadow: 0 0 70px rgba(0, 0, 0, 0.4);
    border: 2px solid grey;
    justify-content: center;
    text-align: center;
    min-height: 30vh;
  }

    h1 {
        margin-bottom: 32px;
    }
}
</style>
