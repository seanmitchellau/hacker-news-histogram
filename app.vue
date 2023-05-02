<script setup lang="ts">
import Histogram from "~/components/Histogram.vue";

</script>

<template>
    <div class="container">
        <div>
            <h1>Hacker News - Histogram</h1>
        </div>

        <Histogram/>
    </div>
</template>

<style lang="scss">
body {
  font-family: Arial, serif;
  background: rgb(174, 222, 238);
  background: radial-gradient(circle, rgba(174, 222, 238, 1) 40%, rgba(148, 175, 233, 1) 100%);

  .container {
    margin: 5rem auto;
    width: 60%;
    padding: 30px;
    background: #fafafa;
    border-radius: 20px;
    box-shadow: 0 0 40px rgba(0, 0, 0, 0.3);
    border: 2px solid grey;
    justify-content: center;
    text-align: center;
    min-height: 40vh;
  }
}
</style>
