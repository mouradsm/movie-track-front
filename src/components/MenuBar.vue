
<template>
    <div class="px-4 flex justify-between fixed bottom-0 bg-[#248588] w-full h-12 text-white" >
        <button type="button" @click="$emit('changepage', 'movies' )">Movies</button>
        <button type="button" @click="$emit('changepage', 'tvshows' )">Tv Shows</button>
        <button type="button" ><a href="/signin">Profile</a></button>
    </div>
</template>

<script setup>

</script>

