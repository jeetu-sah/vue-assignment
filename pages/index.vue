<template>
  <main id="main-content" class="min-h-screen bg-gray-100 p-6" role="main">
    <!-- Skip to content link (for keyboard users) -->
    <a href="#main-content"
      class="sr-only focus:not-sr-only focus:absolute focus:top-2 focus:left-2 bg-white text-blue-700 p-2 rounded">
      Skip to main content
    </a>

    <header role="banner">
      <h1 class="text-4xl font-bold text-center mb-8" tabindex="0">Movie List</h1>
    </header>

    <!-- Error message -->
    <div v-if="error" class="text-red-500 text-center" role="alert" aria-live="assertive">
      {{ error }}
    </div>

    <!-- Loading message -->
    <div v-if="loading" class="text-center" role="status" aria-live="polite">
      Loading...
    </div>

    <!-- Movie grid -->
    <section v-else class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
      aria-label="Movie results">
      <NuxtLink v-for="movie in movies" :key="movie.imdbID" :to="`/movie/${movie.imdbID}`"
        class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow focus:outline focus:outline-2 focus:outline-blue-600"
        :aria-label="`View details for ${movie.Title}`">
        <img :src="movie.Poster" :alt="`${movie.Title} movie poster`" class="w-full h-60 object-cover" />
        <div class="p-4">
          <h2 class="text-xl font-semibold" tabindex="0">{{ movie.Title }}</h2>
          <p class="text-gray-600 text-sm mt-1" tabindex="0">Year: {{ movie.Year }}</p>
        </div>
      </NuxtLink>
    </section>
  </main>
</template>


<script setup>
import { ref, onMounted } from 'vue'
import { useRuntimeConfig } from '#app'
const config = useRuntimeConfig()
const API_KEY = config.public.omdbApiKey

const searchQuery = ref('Batman')
const movies = ref([])
const error = ref(null)
const loading = ref(false)

const fetchMovies = async () => {
  loading.value = true
  error.value = null

  try {
    const res = await fetch(`https://www.omdbapi.com/?apikey=${API_KEY}&s=${encodeURIComponent(searchQuery.value)}`)
    const data = await res.json()
    if (data.Search) {
      movies.value = data.Search
    } else {
      error.value = data.Error || 'No results found'
      movies.value = []
    }
  } catch {
    error.value = 'Failed to fetch movies'
  } finally {
    loading.value = false
  }
}

onMounted(fetchMovies)
</script>
