<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'u-bg-warm' : ''">
    <main>
      <div class="c-input">
        <input 
        type="text"
        placeholder="Search.." 
        class="c-input__search"
        v-model="query"
        @keypress="fetchWeather"/>
      </div>
      <div class="c-weather">
        <div class="c-weather__wrapper" v-if="typeof weather.main != 'undefined'">
          <div class="c-weather__info">
            <div class="c-weather__location">{{weather.name}},{{ weather.sys.country }}</div>
            <div class="c-weather__date">{{dateBuilder()}}</div>
            <div class="c-weather__temp">{{Math.round(weather.main.temp)}}º</div>
            <div class="c-weather__weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',  
  data(){
    return {
      api_key: 'b2940eaf99e66adc30412425262c787f',
      api_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      if (e.key == "Enter"){
        fetch(`${this.api_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
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
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;

}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s ease all;
}
#app.u-bg-warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.c-weather{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap:24px;
}
.c-weather__info{
  text-align: center;

}
.c-input{
  padding-bottom: 16px;
}
.c-input__search{

  display: block;
  width: 100%;
  padding: 15px;

  color:#313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s ease all;
}

.c-input__search:focus{
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius:  16px 0 16px 0;
}

.c-weather__location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.c-weather__date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}



.c-weather__temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.c-weather__weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>

