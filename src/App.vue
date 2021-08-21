<template>
  <div id="app" :class="typeof api.main != 'undefined' && api.main.temp > 16 ? 'warm' : ''">
    <main class="weather-app">
      <div class="weather-app-search">
        <input 
          type="text" 
          class="weather-app-search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchApi"
        >
      </div>

      <div class="weather-app__content" v-if='typeof api.main != "undefined"'>
        <div class="weather-app-location">
          <div class="weather-app-location__text">
            {{ api.name }}
            <span>{{ api.sys.country }}</span>
          </div>
          <div class="weather-app-location__date">{{ currentDate() }}</div>
        </div>

        <div class="weather-app-info">
          <div class="weather-app__temp">{{ api.main.temp }}°C</div>
          <div class="weather-app__weather">{{ api.weather[0].main }}</div>
          <img :src="'http://openweathermap.org/img/wn/' + `${api.weather[0].icon}@2x.png`" /> 
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '054b4b86484b6cfb44dcf52e8310208d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      api: {},
      timestamp: ''
    }
  },
  created() {
    fetch('https://api.openweathermap.org/data/2.5/weather?q=hanoi&appid=054b4b86484b6cfb44dcf52e8310208d&units=metric')
      .then(response => response.json())
      .then(data => this.api = data);
  },
  methods: {
    fetchApi(e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}&units=metric`)
          .then(response => response.json())
          .then(this.setResults);
        this.query = '';
      }
    },
    setResults(result) {
      this.api = result;
      console.log(result);
    },
    currentDate() {
      const current = new Date();
      const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`;
      return date;
    }
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

.weather-app {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));;
}

.weather-app-search{
  width: 100%;
  margin-bottom: 30px;
}
  
.weather-app-search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  padding: 15px;

  appearance: none;
  outline: none;
  border: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(225, 225, 225, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.weather-app-search-bar:focus {
  border: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(225, 225, 225, 0.75);
  border-radius: 16px 0 16px 0px;
}

.weather-app-location__text {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);

  position: relative;
}

.weather-app-location__text span {
  position: absolute;
  transform: translateX(6px);

  font-size: 15px;
  color: #fff;

  padding: 3px;

  background-color: rgba(225, 225, 225, 0.5);
  border-radius: 100%;
}

.weather-app-location__date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-app-info {
  text-align: center;
}

.weather-app__temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 60px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(225, 225, 225, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-app__weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media screen and (min-width: 768px) {
  .weather-app-search {
    width: 400px;
    margin: 0 auto 30px;
  }
  .weather-app__temp {
    font-size: 102px;
  }
}
</style>
