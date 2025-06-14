<template>
  <div class="min-h-screen bg-gray-100 p-6">
    <h1 class="text-4xl font-bold text-center mb-8">Movie List</h1>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
      <div v-for="movie in movies" :key="movie.id" class="bg-white rounded-lg shadow-md overflow-hidden">
        <img :src="movie.poster" alt="Movie Poster" class="w-full h-60 object-cover" />
        <div class="p-4">
          <h2 class="text-xl font-semibold">{{ movie.title }}</h2>
          <p class="text-gray-600 text-sm mt-1">{{ movie.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
const movies = ref([
  {
    id: 1,
    title: 'Inception',
    poster: 'https://via.placeholder.com/300x400?text=Inception',
    description: 'A mind-bending thriller by Christopher Nolan.',
  },
  {
    id: 2,
    title: 'Interstellar',
    poster: 'https://via.placeholder.com/300x400?text=Interstellar',
    description: 'Space exploration and time dilation drama.',
  },
  {
    id: 3,
    title: 'The Dark Knight',
    poster: 'https://via.placeholder.com/300x400?text=Dark+Knight',
    description: 'Batman faces off against the Joker in Gotham.',
  },
])
</script>
