<template>
    <main id="main-content" class="min-h-screen bg-gray-100 p-6" role="main">
        <!-- Skip link for keyboard users -->
        <a href="#main-content"
            class="sr-only focus:not-sr-only focus:absolute focus:top-2 focus:left-2 bg-white text-blue-700 p-2 rounded">
            Skip to main content
        </a>

        <nav role="navigation" aria-label="Back to movie list">
            <NuxtLink to="/"
                class="text-blue-600 hover:text-blue-800 underline mb-6 inline-block transition focus:outline focus:outline-2 focus:outline-blue-600">
                ← Back to list
            </NuxtLink>
        </nav>

        <!-- Loading state -->
        <div v-if="loading" class="text-center text-xl text-gray-700 font-semibold" role="status" aria-live="polite">
            Loading...
        </div>

        <!-- Error state -->
        <div v-else-if="error" class="text-center text-red-600 font-semibold" role="alert" aria-live="assertive">
            {{ error }}
        </div>

        <!-- Movie detail content -->
        <article v-else class="max-w-4xl mx-auto bg-white rounded-2xl shadow-xl p-6 md:p-10" aria-label="Movie detail">
            <div class="flex flex-col md:flex-row gap-8">
                <img :src="movie?.Poster" :alt="`${movie?.Title} official movie poster`"
                    class="w-full md:w-1/3 object-cover rounded-xl shadow-md" />
                <div class="flex-1 space-y-3">
                    <h1 class="text-4xl font-bold text-gray-800" tabindex="0">
                        {{ movie?.Title }}
                    </h1>
                    <p class="text-sm text-gray-600 italic" tabindex="0">
                        {{ movie?.Genre }} | {{ movie?.Year }} | Rated: {{ movie?.Rated }}
                    </p>
                    <p class="text-gray-700" tabindex="0">
                        <strong>Plot:</strong> {{ movie?.Plot }}
                    </p>

                    <section aria-label="Additional movie details"
                        class="grid grid-cols-1 md:grid-cols-2 gap-4 pt-4 text-sm text-gray-800">
                        <p tabindex="0"><strong>Director:</strong> {{ movie?.Director }}</p>
                        <p tabindex="0"><strong>Actors:</strong> {{ movie?.Actors }}</p>
                        <p tabindex="0"><strong>Writer:</strong> {{ movie?.Writer }}</p>
                        <p tabindex="0"><strong>Released:</strong> {{ movie?.Released }}</p>
                        <p tabindex="0"><strong>Runtime:</strong> {{ movie?.Runtime }}</p>
                        <p tabindex="0"><strong>Language:</strong> {{ movie?.Language }}</p>
                        <p tabindex="0"><strong>Country:</strong> {{ movie?.Country }}</p>
                        <p tabindex="0"><strong>Box Office:</strong> {{ movie?.BoxOffice }}</p>
                        <p tabindex="0"><strong>Awards:</strong> {{ movie?.Awards }}</p>
                        <p tabindex="0"><strong>IMDB Rating:</strong> {{ movie?.imdbRating }}</p>
                        <p tabindex="0"><strong>IMDB Votes:</strong> {{ movie?.imdbVotes }}</p>
                        <p tabindex="0"><strong>Metascore:</strong> {{ movie?.Metascore }}</p>
                        <p tabindex="0"><strong>Production:</strong> {{ movie?.Production }}</p>
                    </section>
                </div>
            </div>
        </article>
    </main>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { useRoute } from 'vue-router'
import { useRuntimeConfig } from '#app'
const config = useRuntimeConfig()
const API_KEY = config.public.omdbApiKey

const route = useRoute()
const movie = ref(null)
const loading = ref(false)
const error = ref(null)

const fetchMovieDetail = async () => {
    loading.value = true
    try {
        const res = await fetch(`https://www.omdbapi.com/?apikey=${API_KEY}&i=${route.params.id}`)
        const data = await res.json()
        if (data.Response === 'True') {
            movie.value = data
        } else {
            error.value = data.Error || 'Movie not found'
        }
    } catch {
        error.value = 'Failed to fetch movie details'
    } finally {
        loading.value = false
    }
}

onMounted(fetchMovieDetail)
</script>
