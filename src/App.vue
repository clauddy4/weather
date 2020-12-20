<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date"> {{ dateBuilder() }} </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
// ?q=London,uk&APPID=e92bad045036be6cc831c4d14e07b2b1

export default {
  name: 'App',
  data() {
    return {
      api_key: '94a008690fbf62a14ec1f1342f6486f9',
      url_base: 'http://api.openweathermap.org/data/2.5/weather',
      query: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json()
        }).then(this.setResult)
      }
    },
    setResult(result) {
      this.weather = result;
    },
    dateBuilder() {
      let now = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

      let day = days[now.getDay()]
      let date = now.getDate()
      let month = months[now.getMonth()]
      let year = now.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
@import './style/main.scss';
</style>
