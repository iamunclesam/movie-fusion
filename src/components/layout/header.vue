<template>
    
    <main class="m-10 rounded">
        <div class="relative w-full h-full">
            <img :src="getBackgroundImageUrl(currentMovie.backdrop_path)" alt="Background Image" class="w-full h-full rounded-md object-cover backdrop-image" />
            <div class="absolute inset-0 overlay">
                
                <transition name="fade" mode="out-in">
                    <div :key="currentMovie.id" class="grid grid-cols md:grid-cols-2 gap-10  md:px-20 md:py-5 p-5">
                        <div class="col-sapn-1 mt-2 md:mt-28">
                            <h1 class="title md:text-5xl text-3xl text-white line-clamp-3">{{ currentMovie.title }}</h1>
                            <!-- ... rest of your content ... -->
                            <button type="button"
                            class="py-1 px-4 mr-2 my-4 mb-2 text-sm font-medium focus:outline-none text-white rounded-full border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700">16+</button>

                        <div class="flex items-center my-5">
                            <svg class="w-4 h-4 text-yellow-300 mr-1" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                                fill="currentColor" viewBox="0 0 22 20">
                                <path
                                    d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                            </svg>
                            <p class="ml-2 text-sm font-bold text-white dark:text-white"> {{ formattedRating }} </p>
                            <span class="w-1 h-1 mx-1.5 bg-gray-500 rounded-full dark:bg-gray-400"></span>
                            <a href="#"
                                class="text-sm font-medium text-white hover:no-underline dark:text-white"> {{ formatReleaseDate(currentMovie.release_date) }}</a>
                        </div>

                        <p class="text-white text-sm line-clamp-2">
                           {{currentMovie.overview}}
                        </p>


                            <div class="flex gap-8">
                                <button @click="goToDetails(currentMovie.id)"
                                    class="bg-red-700 my-4 text-white text-sm rounded-md flex py-2 px-8 shadow-lg">
                                   See Movie
                                    <Icon icon="solar:play-bold" class="mt-1 ml-1" />
                                </button>
<!-- 
                                <button type="button"
                                    class="w-10 h-10 mr-2 my-4 mb-2 text-sm font-medium text-gray-900 focus:outline-none rounded-full border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-200 dark:focus:ring-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700">
                                    <Icon icon="teenyicons:add-outline" width="20" color="white" class="mx-auto" />
                                </button> -->
                            </div>
                        </div>

                        <div class="col-span-1 col-end md:p-20 md:pt-20 mx-auto">
                            <div class="flex h-auto items-center justify-center">
                                <img :src="getImageUrl(currentMovie.poster_path)" class="hidden md:hidden lg:block object-cover" alt="">
                            </div>
                        </div>
                    </div>
                </transition>
            </div>
        </div>


        <TopRatedActorsVue />
    </main>
</template>
  
<script>
import { Icon } from '@iconify/vue';
import navbarVue from './navbar.vue';
import axios from 'axios';
import TopRatedActorsVue from '../TopRatedActors.vue';


export default {
    components: { Icon, navbarVue, TopRatedActorsVue },
    data() {
        return {
            currentMovie: {},
            tmdbRuntime: null,
            tmdbRating: null,
        };
    },

    computed: {
        formattedDate() {
            const date = this.movie.release_date
            const formatDate = format(date, 'MMMM d, yyyy');
            return formatDate
        },

        formattedRuntime() {
            return this.tmdbRuntime !== null ? this.formatRuntime(this.tmdbRuntime) : '';
        },

        formattedRating() {
            return this.tmdbRating !== null ? this.formatRating(this.tmdbRating) : '';
        },
    },


    mounted() {
        this.fetchRandomMovie();
        setInterval(this.fetchRandomMovie, 100000); 
    },
    methods: {
        getImageUrl(path) {
            return path ? `https://image.tmdb.org/t/p/w300${path}` : 'https://via.placeholder.com/500';
        },

        getBackgroundImageUrl(path) {
            return path ? `https://image.tmdb.org/t/p/w1280${path}` : 'https://via.placeholder.com/500';
        },

        fetchRandomMovie() {

            const url = ' https://api.themoviedb.org/3/movie/now_playing';
            const options = {
                method: 'GET',
                headers: {
                    accept: 'application/json',
                    Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIwMzY0YzQ2YWI4NjQyN2Q1YzM2ZWNmOTU3MGIwMjQ0OSIsInN1YiI6IjY0OTZjODYzZWI3OWMyMDBjNTZkNDIwNiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.B1why-oEZuEzU5xSfBffmEPZt7_xwFrVoaPUi_O26Ds'
                }
            };

            axios.get(url, options)
                .then(response => {
                    const data = response.data;
                    const randomIndex = Math.floor(Math.random() * data.results.length);
                    this.currentMovie = data.results[randomIndex];
                    this.tmdbRuntime = data.results[randomIndex].runtime;
                    this.tmdbRating =  data.results[randomIndex].vote_average;

                })

                .catch(err => {
                    console.log('Error fetching movie', err);
                })
        },


        formatReleaseDate(dateString) {
            try {
                const date = new Date(dateString);
                const options = { day: 'numeric', month: 'long', year: 'numeric' };
                const formattedDate = date.toLocaleDateString(undefined, options);
                return formattedDate.replace(/(\d+)([a-z]+)/i, (_, day, month) => `${this.addSuffix(day)} ${month}`);
            } catch (error) {
                console.error('Error formatting release date:', error);
                return 'Invalid Date';
            }
        },
        addSuffix(day) {
            if (day >= 11 && day <= 13) {
                return `${day}th`;
            }
            switch (day % 10) {
                case 1: return `${day}st`;
                case 2: return `${day}nd`;
                case 3: return `${day}rd`;
                default: return `${day}th`;
            }
        },

        formatRuntime(minutes) {
            const hours = Math.floor(minutes / 60);
            const remainingMinutes = minutes % 60;
            return `${hours}h ${remainingMinutes}m`;
        },


        formatRating(rating) {
            if (rating !== null && !isNaN(rating)) {
                const formattedRating = (rating).toFixed(1); // Format to one decimal place
                return `${formattedRating}/10`;
            } else {
                return 'N/A'; // or any other default value for cases where the rating is not available or not a valid number
            }
        },

        goToDetails(id) {
            this.$router.push(`/movie/details/${id}`)
        }


    },
};
</script>
  
<style scoped>
.nav {
    display: relative;
    z-index: 2;
}

.overlay {
    background: rgba(0, 0, 0, 0.73);
    border-radius: 10px;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

.backdrop-image {
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    filter: blur(8px); /* Adjust the blur effect */
    animation: fadeIn 1.5s ease; /* Adjust the animation duration and timing function */
    border-radius: 10px;
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@media screen and (max-width: 780px) {
    .backdrop-image {
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    filter: blur(2px); /* Adjust the blur effect */
    animation: fadeIn 1.5s ease; /* Adjust the animation duration and timing function */
}
}

</style>
  
