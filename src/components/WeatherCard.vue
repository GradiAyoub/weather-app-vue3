<script setup>
import BorderLine from './BorderLine.vue'
import WeatherForecastDay from './WeatherForecastDay.vue';
import WeatherInfo from './WeatherInfo.vue';
import {ref} from 'vue'




defineProps({
    location: Object
})

const emit= defineEmits(['delete-location'])
const showDetail= ref(false)

const removeLocation= (locationName)=>{
  emit('delete-location', locationName)
  showDetail.value= false
}

</script>

<template>
  <div :class="location.current.is_day ===1? 'bg-day': 'bg-night'" class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden">
    <!-- Location & time -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ location.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">{{ new Date(location.location.localtime).getHours() }} </h1>
        {{ new Date(location.location.localtime).getMinutes() }}
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img :src="location.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="text-9xl mb-2">{{ Math.round(location.current.temp_c)}}&deg;</h1>
      <p class="text-2xl">{{ location.current.condition.text }}</p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div v-for="(day, id) in location.forecast.forecastday" :key="id">
      <WeatherForecastDay :day="day" />
    </div>

    <!-- info -->
    <Transition name="v">
      <div v-show="showDetail">
        <WeatherInfo :location="location" @close-info="showDetail=false"
        @remove-location="removeLocation(location.location.name)"  />
      </div>


    </Transition>
   
    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="showDetail= true">More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i></button>
    </div>
  </div>
</template>


<style scoped>

.bg-day {
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
.bg-night {
  background-color: #07223d;
  background-image: linear-gradient(62deg, #0a2a4a 0%, #270845 100%);
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

</style>
