/* eslint-disable */
<template>
  <div id="App" v-bind:class="typeof state.weather.main !== 'undefined' && state.weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="search..."
          v-model="query"
          @keypress="fetchinfo"
        />
      </div>

      <div class="weather-wrap" v-if="typeof state.weather.name !== 'undefined'">
        <div class="location-box">
          <div class="location">{{state.weather.name}}, {{ state.weather.sys.country }}</div>
          <div class="date">{{ dateBuilder ()}}</div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(state.weather.main.temperature) }}c</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { reactive } from "vue";
const state = reactive({
  api_key: "fcbb2bc86ba6a9dee387dedc4d3734d3",
  url_base: 'https://api.openweathermap.org/data/2.5/',
  query: "",
  weather: {},
});


const setResults = (results) => {
  state.weather = results;
  console.log(state.weather);
};

const fetchinfo = async (e) => {
  if(e.key ==="Enter"){
    fetch(`${state.url_base}weather?q=${state.query}&units=metric&APPID=${state.api_key}`)
    .then(res => {
      return res.json();
    }).then(results => {
      setResults(results);
      console.log(results);
    })
  }
};

const dateBuilder = () => {
  let d= new Date();
  let months = ["January", "February", "March", "April", "May", "June", "July","August", "September", "October", "November", "December",
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

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

#App {
  background-image: url("https://github.com/TylerPottsDev/weather-vue/blob/master/src/assets/cold-bg.jpg?raw=true");
  background-size: cover;
  background-position: bottom;
}

#App.warm{
  background-image: url("https://github.com/TylerPottsDev/weather-vue/blob/master/src/assets/warm-bg.jpg?raw=true");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 8px, 16px, 8px, 16px;
  transition: 0.4s;
  border: none;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0 0 0 2px rgba(0, 0, 0, 0.25);
  border-radius: 16px, 0px, 16px, 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 300;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
</style>
