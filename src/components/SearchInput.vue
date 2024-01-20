<script setup>
import { reactive } from 'vue';

const emit = defineEmits(['location-data'])

  const searchTerm= reactive({
    query: '',
    timeout: null,
    results: null
  })

  const handleSearch= ()=>{
    clearTimeout(searchTerm.timeout)
    searchTerm.timeout= setTimeout(async ()=>{
      if(searchTerm.query !==''){
        const res= await fetch(`http://api.weatherapi.com/v1/search.json?key=166987f0a9004855ac0125701241301&q=${searchTerm.query}`)
        const data= await res.json()
        searchTerm.results= data
      }else{
        searchTerm.results= null
      }
    
    },500)
  }

  const getWeather= async(id)=>{
    const res= await fetch(`http://api.weatherapi.com/v1/forecast.json?key=166987f0a9004855ac0125701241301&q=id:${id}&days=3&aqi=no&alerts=no`)
    const data= await res.json()
    emit('location-data', data)
    searchTerm.results= null
  }

</script>

<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query" @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg" v-if="searchTerm.results !== null">
      <div v-for="location in searchTerm.results" :key="location.id">
        <button @click="getWeather(location.id)" class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
          {{ location.name }},   {{ location.region }},  {{ location.country }},
        </button>
      </div>
    </div>
  </div>
</template>
