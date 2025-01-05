<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
        <h1>Home</h1>
        <button type="button" class="btn btn-primary" @click="pickCards">Bốc bài</button>
        </div>
        <div class="cards container">
        <h2>Các lá bài của bạn là: </h2>
        <div class="row align-items-center">
            <div class="col" v-for="(card, index) in cards" :key="index">
                <p>{{ card.name }}</p>
                <img :src="card.src" alt="card.name" />
                <p>{{ card.mainMeaning }}</p>
            </div>
        </div>
    </div>
    <div>
        <button type="button" class="btn btn-danger" v-if="Object.keys(cards).length > 0" @click="getResult">
            Đọc kết quả
        </button>
    </div>
    <div class="result" v-if="Object.keys(result).length > 0">
        <h2>Kết quả: </h2>
        <p>{{ result }}</p>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';

const cards = ref([]);
const result = ref([]);


const pickCards = () => {
    axios.get('http://127.0.0.1:8000/api/pick-cards')
        .then((response) => {
            cards.value = response.data;
            console.log(cards.value);
        })
        .catch((error) => {
            console.error(error);
        });
};

const getResult = () => {
    const payload = { cards: Object.keys(cards.value) };
    axios.post('http://127.0.0.1:8000/api/get-result', payload)
        .then((response) => {
            result.value = response.data;
        })
        .catch((error) => {
            console.error(error);
        });
};

</script>

<style scoped>
.home {
}
</style>
