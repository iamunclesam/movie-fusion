<template>
    <main class="mb-20 md:mb-0">
        <nav class="fixed top-0 mb-46 md:mb-0 w-full z-10 bg-white sm:hidden border-b  border-gray-200 dark:border-gray-700">
            <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
                <div class="icon">
                    <Icon icon="ion:arrow-back" width="25" class="text-gray-500" @click.prevent="goBack" />
                </div>
                <router-link to="/" class="flex justify-center items-center">
                    <span
                        class="self-center text-red-700  md:text-white text-2xl font-semibold whitespace-nowrap dark:text-white">Movie-Fusion</span>
                    <!-- <img src="../../assets/logo-1.png" class=" self-center w-full h-10 p-0 m-0" alt=""> -->
                </router-link>
                <button data-collapse-toggle="navbar-dropdown" type="button"
                    class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
                    aria-controls="navbar-dropdown" aria-expanded="false">
                    <span class="sr-only">Open main menu</span>
                    <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                        viewBox="0 0 17 14">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M1 1h15M1 7h15M1 13h15" />
                    </svg>
                </button>
                <div class="hidden w-full md:block md:w-auto" id="navbar-dropdown">
                    <ul
                        class="flex flex-col font-medium p-4 md:p-0 mt-4 border border-gray-100 rounded-lg md:flex-row md:space-x-8 md:mt-0 md:border-0 dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">
                        <li>
                            <router-link to='/'
                                class="flex block py-2 mt-1 pl-3 pr-4 md:text-white  rounded md:bg-transparent  md:p-0 md:dark:text-blue-500 dark:bg-blue-600 md:dark:bg-transparent"
                                aria-current="page">
                                <Icon icon="majesticons:home" class="mt-1 mr-2" /> Home
                            </router-link>
                        </li>
                        <li>
                            <button id="dropdownNavbarLink" data-dropdown-toggle="dropdownNavbar"
                                class="flex mt-1 md:text-white items-center justify-between w-full py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 md:w-auto dark:text-white md:dark:hover:text-blue-500 dark:focus:text-white dark:border-gray-700 dark:hover:bg-gray-700 md:dark:hover:bg-transparent">
                                <span class="flex">
                                    <Icon icon="material-symbols:action-key" class="mt-1 mr-2" />
                                    Genres
                                </span>
                                <svg class="w-2.5 h-2.5 ml-2.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                                    fill="none" viewBox="0 0 10 6">
                                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                                        stroke-width="2" d="m1 1 4 4 4-4" />
                                </svg>
                            </button>
                            <!-- Dropdown menu -->
                            <div id="dropdownNavbar"
                                class="z-10 hidden font-normal divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600">
                                <ul class="py-2 text-sm text-gray-700 dark:text-gray-400"
                                    aria-labelledby="dropdownLargeButton">
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
                                </ul>
                                <div class="py-1">
                                    <a href="#"
                                        class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-400 dark:hover:text-white">Sign
                                        out</a>
                                </div>
                            </div>
                        </li>
                        <li>
                            <router-link to='/movies'
                                class="flex mt-1 md:text-white py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                                <Icon icon="ic:baseline-local-movies" class="mt-1 mr-2" />
                                Movies
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/my-collections"
                                class="flex mt-1  md:text-white py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                                <Icon icon="mdi:collection" class="mt-1 mr-2" />
                                Collections
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/profile"
                                class="md:hidden flex mt-1  md:text-white py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                                <Icon icon="iconamoon:profile-fill" class="mt-1 mr-2" />
                                Profile
                            </router-link>
                        </li>

                        <li>
                            <router-link to="/movies/collection"
                                class="md:hidden flex mt-1  md:text-white py-2 pl-3 pr-4 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">
                                <Icon icon="uis:signout" class="mt-1 mr-2" />
                                Sign Out
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/signin" type="button"
                                class="hidden md:block md:text-white bg-red-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded text-sm px-4 py-2 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Sign
                                in</router-link>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </main>
</template>

<script>
import { Icon } from '@iconify/vue';
export default {
    components: { Icon },
    methods: {
        goBack() {
            this.$router.go(-1);
        },
    },
}
</script>

<style></style>