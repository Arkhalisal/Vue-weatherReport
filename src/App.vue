<script setup>
import { onMounted, ref } from "vue";
import moment from "moment";
import WeatherImage from "./components/WeatherImage.vue";

const weatherData = ref(null);

onMounted(async () => {
  try {
    const data = await fetch("https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=tc");
    const res = await data.json();
    weatherData.value = res;
    console.log(weatherData.value);
  } catch (err) {
    console.error(err);
  }
});
</script>

<template>
  <div class="min-h-screen bg-gradient-to-b from-blue-100 to-blue-300 py-8 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto bg-white rounded-lg shadow-xl overflow-hidden">
      <!-- title -->
      <h1 class="text-3xl font-bold text-center text-blue-800 py-6 bg-blue-200">香港九天天氣預報</h1>

      <!-- general situation -->
      <div class="text-lg text-gray-700 px-6 py-4 bg-blue-50 border-b border-blue-200">
        {{ weatherData?.generalSituation }}
      </div>

      <!-- weather list -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 p-6">
        <div
          v-for="day in weatherData?.weatherForecast"
          :key="day"
          class="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition duration-300 flex flex-col justify-around"
        >
          <!-- date and day -->
          <div class="flex justify-between items-center mb-2">
            <div class="text-lg font-semibold text-blue-700">{{ moment(day.forecastDate).format("MM/DD") }}</div>
            <div class="text-sm font-medium text-gray-600">{{ day.week }}</div>
          </div>

          <!-- wind and weather -->
          <div class="flex flex-row justify-between w-full">
            <div class="flex flex-col w-5/6">
              <div class="text-sm text-gray-600 mb-2">{{ day.forecastWind }}</div>
              <div class="text-base font-medium text-gray-800 mb-2">天氣: {{ day.forecastWeather }}</div>
            </div>
            <WeatherImage :pic-id="day.ForecastIcon" class="w-[48px]" />
          </div>

          <!-- temperature -->
          <div class="flex justify-between items-center mb-2">
            <div class="text-sm">
              <span class="text-blue-600">最低 </span>
              <span class="font-bold">{{ day.forecastMintemp.value }}°C</span>
            </div>
            <div class="text-sm">
              <span class="text-red-600">最高 </span>
              <span class="font-bold">{{ day.forecastMaxtemp.value }}°C</span>
            </div>
          </div>

          <!-- humidity -->
          <div class="flex flex-row justify-between items-center">
            <div class="text-sm text-gray-600">
              相對濕度: {{ day.forecastMinrh.value }}% - {{ day.forecastMaxrh.value }}%
            </div>
            <div>
              降雨概率:<span class="font-extrabold">{{ day.PSR }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
