
<template>
    <div class="weather-container">
      <div class="header">
        <img class="sun-img" src="../assets/sun.png" alt="sun">
        <h1 class="heading">
          Weather App
        </h1>
      </div>
      <div class="form-container">
        <form v-on:submit.prevent="getData" class="weather-form">
          <input v-model="city" type="text" placeholder="Search for a city" autofocus required>
          <div>
            <button type="submit" >GET WEATHER</button>
            <button type="button" v-on:click="clearAll">CLEAR ALL</button>
          </div>
        </form>
      </div>
      <div class="data-container">
          <div class="weather-card">
            <ul class="cities" >
                <li class="city" v-for="result in weatherResults" v-bind:key="result.id">
                    <h2 class="city-name">
                        <span>{{result.name}}</span>
                        <sup>{{result.sys.country}}</sup>
                    </h2>
                    <div class="city-temp">{{Math.round(result.main.temp)}}<sup>°C</sup></div>
                    <div class="icon-desc">
                      <div class="icon-info">
                          <img class="city-icon" v-bind:src="'https://openweathermap.org/img/wn/' + result.weather[0].icon + '@2x.png' "  />
                          <div class="hum-wind">
                            <p>Hum: {{ result.main.humidity }} %</p>
                            <p>Wind: {{ result.wind.speed }} m/s</p>
                          </div>
                      </div>
                      <div >
                        <p class="desc">{{result.weather[0].description}}</p>
                      </div>
                    </div>
                </li>
            </ul>
          </div>
      </div>
      <footer class="weather-footer">
        <div>
          <a href="https://github.com/Can-Bin" target="_blank"><img src="../assets/github.png" alt="github"></a>
          <a href="https://www.linkedin.com/in/can-bin/" target="_blank"><img src="../assets/linkedin.png" alt="linkedin"></a>
        </div>
        <small>Coded by <span>&#9733;</span> Can BİN</small>
      </footer>
    </div>
  </template>

  <script>
  export default {
    name: 'WeatherApp',
    data () {
      return {
        weatherResults: [],
        apiKey: process.env.weatherAppId,
        city:''
      }
    },

    methods:{
       async getData(){
            await fetch (`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}&units=metric`)
            .then(res=> res.json())
            .then((data) => {
              if(data.cod == 200){
                this.weatherResults.push(data)
                this.city=""
                console.log(this.weatherResults)
              }else{
                alert(data.message)
                this.city=""
              }
            })
        },

        clearAll(){
          this.weatherResults = []
        }

    }

  }
  </script>

  <style scoped src="../assets/weather.css">

  </style>
