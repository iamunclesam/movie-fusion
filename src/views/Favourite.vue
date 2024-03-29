<template>
    <div class="navbar hidden sm:block">
        <navbarVue class="" />
    </div>

    <div class="previousNav">
        <previousNavVue />
    </div>

    <main class="px-5">
        <div class="genre flex justify-between mt-5">
            <h1 class="text-left text-gray-900 text-2xl">Favourites</h1>
            <Icon icon="ph:pencil" width="20" />
        </div>

        <div class="sort my-4 flex">
            <h1 class="text-gray-500 text-sm ">Sort by</h1>

            <button id="dropdownDefaultButton" data-dropdown-toggle="dropdown"
                class="my-0 text-black hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm mx-2 text-center inline-flex items-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                type="button">Suggested <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                    fill="none" viewBox="0 0 10 6">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                </svg>
            </button>

            <!-- Dropdown menu -->
            <div id="dropdown"
                class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownDefaultButton">
                    <li>
                        <a href="#"
                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Dashboard</a>
                    </li>
                    <li>
                        <a href="#"
                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Settings</a>
                    </li>
                    <li>
                        <a href="#"
                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Earnings</a>
                    </li>
                    <li>
                        <a href="#"
                            class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Sign
                            out</a>
                    </li>
                </ul>
            </div>

        </div>




        <div v-for="movie in movies" :key="movie.id"
            class=" my-2 flex items-center bg-white rounded-lg  md:flex-row md:max-w-xl hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700">
            <!-- <MovieCardVue :movie="movie"/> -->
            <img :src="getImageUrl(movie.backdrop_path)" :alt="movie.title" class="w-28 card-image rounded md:rounded-md" />
            <div class="flex flex-col justify-between pt-1.5 leading-normal">
                <h5 class="mb-2 text-sm px-2 font-medium tracking-tight text-gray-900 dark:text-white truncate w-48">{{
                    movie.title }}
                </h5>
                <router-link :to="`/movie/details/${movie.id}`" type="button"
                    class="px-3 py-1 text-xs font-medium text-center inline-flex items-center text-gray-900 bg-white rounded hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                    <Icon icon="solar:play-bold" class="mr-2" />
                    Play
                </router-link>

                <!-- <p class="mb-3 text-sm font-normal text-gray-700 dark:text-gray-400 line-clamp-2">{{ movie.overview }}</p> -->
            </div>
        </div>

        <div class="bottom-nav m-2">

            <BottomNav />
        </div>
    </main>
</template>

<script>
import BottomNav from '../components/layout/bottomNav.vue'
import axios from 'axios'
import navbarVue from '../components/layout/navbar.vue'
import API_ENDPOINTS from '../utils/ApiRoutes'
import MovieCard from '../components/MovieCard.vue';
import { defineComponent } from 'vue'
import { Carousel, Navigation, Slide } from 'vue3-carousel'
import 'vue3-carousel/dist/carousel.css'
import breadcrumbVue from '../components/layout/breadcrumb.vue';
import { Icon } from '@iconify/vue';
import previousNavVue from '../components/layout/smallDevice-layout/previousNav.vue'


export default defineComponent({
    components: { navbarVue, MovieCard, Carousel, Slide, Navigation, breadcrumbVue, Icon, BottomNav, previousNavVue },

    data() {
        return {
            movies: [],
            genre: {},
            genreName: null,
            carouselItemsToShow: 3.5
        }
    },

    mounted() {
        this.fetchGenre(),
        this.fetchGenreName()
        this.handleResize()
        
    },

    beforeDestroy() {
        window.removeEventListener('resize', this.handleResize)
    },

    methods: {
        handleResize() {
            this.carouselItemsToShow = window.innerWidth >= 768 ? 5.5 : 2.5
        },

        getImageUrl(path) {
            return path ? `https://image.tmdb.org/t/p/w300${path}` : 'https://via.placeholder.com/500';
        },

        getBackgroundImageUrl(path) {
            return path ? `https://image.tmdb.org/t/p/w1280${path}` : 'https://via.placeholder.com/500';
        },


        async fetchGenre() {
            try {

                const url = 'https://api.themoviedb.org/3/discover/movie?include_adult=false';
                const genreId = this.$route.params.id;
                const options = {
                    method: 'GET',
                    headers: {
                        accept: 'application/json',
                        Authorization: API_ENDPOINTS.KEY
                    }
                };
                const response = await axios.get(`https://api.themoviedb.org/3/discover/movie?with_genres=${genreId}`, options)
                this.movies = response.data.results;
                console.log(response.data)
            }

            catch (error) {
                console.error('Error Fetching Genre:', error)
            }
        },

        async fetchGenreName() {
            try {
                const genreId = this.$route.params.id;
                const options = {
                    method: 'GET',
                    headers: {
                        accept: 'application/json',
                        Authorization: API_ENDPOINTS.KEY
                    }
                };
                const response = await axios.get(API_ENDPOINTS.MOVIE_GENRE, options)
                const genre = response.data.genres.find(genre => genre.id == genreId)
                this.genreName = genre
                console.log(genre);
            }

            catch (error) {
                console.error('Error Fetching Genre:', error)
            }
        },

        
    }
})
</script>

<style>
.carousel__item {
    margin: 10px;
    text-align: left !important;
}

@media screen and (max-width: 780px) {
    .carousel__item {
        margin: 2px;
        text-align: left !important;
    }
}
</style>