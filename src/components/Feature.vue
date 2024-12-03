<script setup>
import axios from "axios";
import { ref, onMounted } from 'vue';

const response = ref(null);
const numbers = ref([]);

numbers.value = (() => {
  const set = new Set();

  while (true) {
    set.add(Math.floor(Math.random() * 19));

    if (set.size === 3) {
      return set;
    }
  }
})();

onMounted(async () => {
  response.value = await axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${import.meta.env.VITE_TMDB_KEY}`);
})
</script>

<template>
<div class="text">Featured Movies</div>
  <div v-if="response" class="movie-card">
    <div v-for="number in numbers" :key="response.data.results[number].id" class="movie-card">
      <img :src="`https://image.tmdb.org/t/p/w500${response.data.results[number].poster_path}`" alt="Movie Poster" class="movie-poster" />
      <p class="movie-title">{{ response.data.results[number].title }}</p>
    </div>
  </div>
</template>

<style scoped>
.movie-card {
  display: flex;
  flex-direction: row;
  width: 100%;
  gap: 2%;
  margin-top: 4%;
  margin-bottom: 8%;
  align-items: center;
  position:relative;
}

.FeaturedMovies {
  display: flex;
  gap: 2%;
  flex-direction: row;   
}

.movie-card img {
  flex: 1;
  width: 22.5%;
  height: auto;
  object-fit: cover;
}

.movie-title {
  font-size: 1.1rem;
  margin-top: 10px;
  color: hotpink;
  vertical-align: middle;
}
</style>
