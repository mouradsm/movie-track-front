<script setup>
import MediaCard from '@/components/MediaCard.vue';
import MenuBar from '@/components/MenuBar.vue';
import SearchBar from '@/components/SearchBar.vue';
import axios from 'axios'
import { reactive, onMounted, ref } from 'vue'

const data = reactive({})
const filteredData = reactive({})
let searchTerm = ''
let teste = 'movies'

const fetchData = async (page) => {
  teste = page
  const response = await axios.get('http://localhost/api/' + page)
  data.value = response.data
  filteredData.value = response.data
  
}

const filterByDuration = () => {

  if (!searchTerm) {
    return filteredData.value
  }
  const durationFilter = parseInt(searchTerm)
  filteredData.value = data.value.filter(item => item.duration < durationFilter)
}

const handleInput = (test) => {
  const [hours=0, mins=0] = test.split(':').map(Number)
  searchTerm = hours * 60 + mins
  console.log(searchTerm)
  
  filterByDuration()
}

</script>

<template>
  <SearchBar @search="handleInput"/>
  <div class="bg-[#263537] py-12 px-5 h-full">

    <div v-if="teste == 'movies'" v-for="movie in filteredData.value">
      <MediaCard 
        :title="movie.media.title" 
        :description="movie.media.description" 
        :duration="movie.duration" 
        :cover="movie.media.cover"
      />
    </div>

    <div v-if="teste == 'tvshows'" v-for="tvshow in filteredData.value">
      <MediaCard 
        :title="tvshow.media.title" 
        :description="tvshow.media.description" 
        :seasons="tvshow.seasons" 
        :episodes="tvshow.episodes" 
        :cover="tvshow.media.cover"
      />
    </div>
    <!-- <div v-if="teste == 'movies'" v-for="movie in filteredData.value">
      <MediaCard 
        :title="movie.media.title" 
        :description="movie.media.description" 
        :duration="movie.duration" 
        :cover="movie.media.cover"
      />
    </div>

    <div v-else-if="teste == 'tvshows'" v-for="tvshow in filteredData.value">
      <MediaCard 
        :title="tvshow.media.title" 
        :description="tvshow.media.description" 
        :seasons="tvshow.seasons" 
        :episodes="tvshow.episodes" 
        :cover="tvshow.media.cover"
      />
    </div> -->

  </div>
  <MenuBar @changepage="fetchData"/>
</template>