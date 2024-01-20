<script setup>
import {ref} from 'vue'
import SearchInput from './components/SearchInput.vue';
import WeatherCard from './components/WeatherCard.vue';





const locations= ref([])
const addLocation= (data)=>{
  locations.value.push(data)
}

const deleteLocation= (name)=> {
  if(confirm('are you sure')){
    locations.value= locations.value.filter(p=> p.location.name !== name)
  }

}

</script>
<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-us', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>

     <!-- Search -->
     <div>
      <SearchInput @location-data="addLocation" />
    </div>

    <!-- weather cards -->
    <div class="m-5 grid grid-cols-3 gap-4">
      <div  v-for="(location,idx) in locations" :key="idx">
      <WeatherCard :location="location" @delete-location="deleteLocation"/>


    </div>
     

    </div >
 
  </main>
</template>