<script setup>
import { ref, onMounted, computed, watch } from "vue";

const api_key = "518ec43a769b3d7b14a3d5b1c476bbf1";
const url = "https://api.openweathermap.org/data/2.5/";
const query = ref("");
const weather = ref([]);

const fetchWeather = (e) => {
  if (e.key == "Enter") {
    fetch(
      `${url}weather?q=${query.value}&units=metric&appid=518ec43a769b3d7b14a3d5b1c476bbf1`
    )
      .then((res) => res.json())
      .then((res) => {
        let fetchedData = res;
        weather.value = fetchedData;
      });
  }
};

const dateBuilder = () => {
  let d = new Date();
  let months = [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December",
  ];
  let days = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
  ];
  let day = days[d.getDay()];
  let date = d.getDate();
  let month = months[d.getMonth()];
  let year = d.getFullYear();
  return `${day} ${date} ${month} ${year}`;
};
</script>

<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'clouds' : 'warm'
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
