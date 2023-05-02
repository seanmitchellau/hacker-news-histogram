<script setup>
import { ref, defineEmits } from 'vue';

const props = defineProps({
    loading: Boolean,
});

const username = ref('');

const emit = defineEmits(['search']);

const search = () => {
    emit('search', username.value);
};
</script>

<template>
    <div>
        <input v-model="username" type="text" placeholder="Enter username" />

        <button @click="search" :disabled="!username || props.loading">View Histogram</button>
    </div>
</template>

<style scoped lang="scss">
div {
    margin: 32px 0;
}

input, button {
    padding: 10px;
    border-radius: 7px;
    border: 2px solid #ccc;
}

input {
    margin-right: 10px;
}

button {
    color: #575757;
    cursor: pointer;
    transition: all 0.2s ease-in-out;

    &:not(:disabled):hover {
        color: #fff;
        background: #575757;
        border-color: #575757;
    }
}
</style>
