<template>
    <div class="flex items-center gap-1" aria-label="IMDB rating" tabindex="0">
        <template v-for="(star, index) in getStars(props.movie?.imdbRating)" :key="index">
            <svg v-if="star === 'full'" class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20"
                aria-hidden="true">
                <path
                    d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.286 3.97a1 1 0 00.95.69h4.21c.969 0 1.371 1.24.588 1.81l-3.406 2.474a1 1 0 00-.364 1.118l1.287 3.97c.3.922-.755 1.688-1.54 1.118l-3.406-2.474a1 1 0 00-1.176 0l-3.406 2.474c-.785.57-1.84-.196-1.54-1.118l1.287-3.97a1 1 0 00-.364-1.118L2.025 9.397c-.783-.57-.38-1.81.588-1.81h4.21a1 1 0 00.95-.69l1.286-3.97z" />
            </svg>
            <svg v-else-if="star === 'half'" class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20"
                aria-hidden="true">
                <defs>
                    <linearGradient id="halfGrad">
                        <stop offset="50%" stop-color="currentColor" />
                        <stop offset="50%" stop-color="transparent" />
                    </linearGradient>
                </defs>
                <path fill="url(#halfGrad)"
                    d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.286 3.97a1 1 0 00.95.69h4.21c.969 0 1.371 1.24.588 1.81l-3.406 2.474a1 1 0 00-.364 1.118l1.287 3.97c.3.922-.755 1.688-1.54 1.118l-3.406-2.474a1 1 0 00-1.176 0l-3.406 2.474c-.785.57-1.84-.196-1.54-1.118l1.287-3.97a1 1 0 00-.364-1.118L2.025 9.397c-.783-.57-.38-1.81.588-1.81h4.21a1 1 0 00.95-.69l1.286-3.97z" />
            </svg>
            <svg v-else class="w-5 h-5 text-gray-300" fill="currentColor" viewBox="0 0 20 20" aria-hidden="true">
                <path
                    d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.286 3.97a1 1 0 00.95.69h4.21c.969 0 1.371 1.24.588 1.81l-3.406 2.474a1 1 0 00-.364 1.118l1.287 3.97c.3.922-.755 1.688-1.54 1.118l-3.406-2.474a1 1 0 00-1.176 0l-3.406 2.474c-.785.57-1.84-.196-1.54-1.118l1.287-3.97a1 1 0 00-.364-1.118L2.025 9.397c-.783-.57-.38-1.81.588-1.81h4.21a1 1 0 00.95-.69l1.286-3.97z" />
            </svg>
        </template>

        <span class="ml-2 text-gray-700">
            {{ props.movie?.imdbRating }}/10
        </span>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { useRoute } from 'vue-router'
import { useRuntimeConfig } from '#app'
const config = useRuntimeConfig()
const API_KEY = config.public.omdbApiKey

const props = defineProps({
    movie: {
        type: [Object],
        required: true
    }
})

const route = useRoute()
const movie = ref(null)
const loading = ref(false)
const error = ref(null)

const getStars = (rating) => {
    if (!rating) return []
    const fullStars = Math.floor(Number(rating))
    const halfStar = Number(rating) % 1 >= 0.5 ? 1 : 0
    const emptyStars = 10 - fullStars - halfStar
    
    return [
        ...Array(fullStars).fill('full'),
        ...Array(halfStar).fill('half'),
        ...Array(emptyStars).fill('empty')
    ]
}
</script>