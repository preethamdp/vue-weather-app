<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp >16 ?'warm':''">
    <main>
      <div class="search-box">
        <input type="text" name="" id="" class="search-bar" 
        v-model = "query"
        placeholder="Search..."
        @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">
          {{ weather.name }},{{ weather.sys.country }}
        </div>
        <div class="date">
          {{ dateBuilder() }}
        </div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
      </div>
    </main>
    </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'app',
  data () {
    return {
      api_key: 'dbca700152c427cfa57a85d9065c6403',
      url_base: 'api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather1 (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            console.log(res);
            return res.json();
          }).then(this.setResults);
      }
    },
    async fetchWeather(e){
      if (e.key == "Enter") {
      const { data } = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.api_key}`);
      console.log(data)
      this.setResults(data);
      }
    }
    ,
    setResults (results) {
      this.weather = results;
      this.query = ''
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
*{
  margin:0;
  padding :0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat',sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition:0.4s;
}
#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75))
}
.search-box{
  width: 100%;
  margin-bottom: 13px;
}
.search-bar{
  display: block;
  width : 100%;
  padding: 14px;;
  border-radius: 0px 16px 0px 16px ;
  color:#313131;
  border : none;
  appearance: none;
  background:none;
  outline: none;
  font-size: 14px;
  background-color: rgba(255,255,255,0.5);
  transition : 0.4s;
}

.search-box .search-bar:focus{
  background-color: rgba(255,255,255,0.75);
  border-radius:16px 0px 16px 0px;
  transition:0.4s;
}

.location-box .location{
  width:100%;
  color:#FFF;
  text-align: center;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,0.25)
}
.location-box .date{
  width:100%;
  color:#FFF;
  text-align: center;
  font-size: 16px;
  font-style: italic;
  font-weight: 100;
  opacity: 0.8;
  text-shadow: 1px 3px rgba(0,0,0,0.25)
  
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  color:#FFF;
  display: inline-block;
  font-size: 102px;
  font-weight: 900;
  padding: 10px 25px;
  background-color:rgba(255,255,255,0.25);
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  border-radius:16px;
  margin:30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.3);
  }
  .weather-box .weather{
    color:#FFF;
    font-size:40px;
    text-shadow: 2px 4px rgba(0,0,0,0.25);
    font-weight: 700;
    font-style:italic;
  }

</style>
