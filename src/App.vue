<script setup>
import { RouterLink, RouterView } from 'vue-router';
</script>

<template>
    <nav class=" shadow-md px-6 py-3 flex gap-6 items-center bg-red-600">
        <RouterLink
            to="/"
            class="text-gray-800 font-semibold hover:text-white transition-colors"
        >Home</RouterLink>
        <RouterLink
            to="/about"
            class="text-gray-800 font-semibold hover:text-white transition-colors"
        >About</RouterLink>
        <RouterLink
            to="/login"
            class="text-gray-800 font-semibold hover:text-white transition-colors"
        >Login</RouterLink>
        <RouterLink
            to="/pokemon"
            class="text-gray-800 font-semibold hover:text-white transition-colors"
        >Pokémon</RouterLink>
    </nav>

    <RouterView />
</template>

<style scoped></style>