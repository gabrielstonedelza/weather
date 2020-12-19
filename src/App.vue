<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.weather[0].main == 'Warm' ? 'warm': '' || typeof weather.main != 'undefined' && weather.weather[0].main == 'Mist' ? 'mist': '' || typeof weather.main != 'undefined' && weather.weather[0].main == 'Clouds' ? 'clouds': '' || typeof weather.main != 'undefined' && weather.weather[0].main == 'Clear' ? 'clear': '' || typeof weather.main != 'undefined' && weather.weather[0].main == 'Rain' ? 'rain': '' || typeof weather.main != 'undefined' && weather.weather[0].main == 'Snow' ? 'snow': '' ">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather"/>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }},{{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp)}} degrees </div>
            <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import my_api_key from './key'

export default {
  name: "App",
  data() {
    return {
      api_key: my_api_key,
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      has_results: false
    };
  },
  methods: {

    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`,{method:'get'})
        .then((res)=>{
          const results = res.json()
          return results
        }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results
    },
    dateBuilder(){
      let d = new Date()
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"]
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, Helvetica, sans-serif;
}
#app{
  background-image: url('./assets/cold-background.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/warm-01.jpg');
}

#app.rain{
  background-image: url('./assets/raining.gif');
}

#app.snow{
  background-image: url('./assets/snowing.gif');
}

#app.clear{
  background-image: url('./assets/clear-sky.jpeg');
}
#app.clouds{
  background-image: url('./assets/Cartoon_Clouds_Vector-01.jpg');
}

#app.mist{
  background-image: url('./assets/mist.gif');
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25) ,rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box input{
  min-height: 40px;
  padding: 10px ;
  color: #313131;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0 0 8px rgba(255,255,255,0.25);
  background: none;
  background-color: rgba(255,255,255,0.25);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(255,255,255,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date{
  color: #fff;
  font-size: 18px;
  font-weight: 200;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: bold;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  margin: 30px 0px;
  border-radius: 16px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-style: italic;
  font-weight: 400;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
