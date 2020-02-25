<template>
  <div id="app"
    :class="bgClass">
    <div class="search-box">
      <input 
        type="text" 
        class="search-input"
        placeholder="enter name of the city"
        v-model="location"
        @keypress.enter="sendQuery"
      >
    </div>

      
      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="weather-location">
          <p class="text--city">{{ weather.name }}, {{ weather.sys.country }}</p>
          <p class="text--date">{{ dateNow }}</p>
        </div>

        <p class="text--deg">
          {{ Math.round(weather.main.temp) }} °C 
        </p>

        <p class="text--desc">{{ weather.weather[0].main }} </p>

      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '0dddf2c343d4c66d6f181c30c00b8260',
      url: 'https://api.openweathermap.org/data/2.5/weather?q=',
      location: '',
      bgClass: 'basic',
      weather: {}
    }
  },
  watch: {
      weather: function(newValue) {
       if (typeof newValue.main == 'undefined') { 
          this.bgClass = 'basic'
       } else {
         if (newValue.main.temp > 20) {
          this.bgClass = 'hot'
         } else {
          this.bgClass = 'cold'
         } 
       }
    }
  },
  computed: {
    dateNow() {
      const d = new Date();
      const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
      const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
      return `${days[d.getDay()]} ${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()} `
    }
  },
  methods: {
      sendQuery: function() {
        fetch( `${this.url}${this.location}&appid=${this.api_key}&units=metric`)
          .then( res => {
            return res = res.json()
          }).then( res => {
            this.weather = res;
          })
      }
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
    background-size: cover;
  background-position: bottom;
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
  padding: 25px;
  transition: .4s;
}

body {
  font-family: 'Montserrat', sans-serif;
  margin:0;
}

.search-input {
  padding: 25px;
  width: 300px;
  border-radius: 16px 0 16px 0;
  border: none;
  transition: .4s;

  font-size: 16px;
  color: rgba(0,0,0,.75);

  box-shadow:  0px 0px 10px rgba(0, 0, 0, 0.25);
}


.search-input:focus {
  border-radius: 0 16px 0 16px;
}

.text--city {
  font-size: 20px;
  font-weight: 500;
  color: rgba(255,255,255);
  text-shadow:  1px 1px rgba(0, 0, 0, 0.25);
  margin-bottom: 5px;
}

.text--date {
  margin-top: 0;
  font-size: 16px;
  font-weight: 300;
  font-style: italic;
}


.text--deg {
  font-size: 72px;
  color: rgba(255,255,255);
  padding: 20px 25px;
  background-color: rgb(255,255,255,.5);
  display: inline-block;
  margin: 0 auto;
  font-weight: 900;
  border-radius: 16px;

  box-shadow: 2px 5px rgba(0, 0, 0, 0.25);

  transition: .4s;
  }

.text--desc {
  font-size: 36px;
  font-style: italic;
  font-weight: 700;
  color: rgba(255,255,255);
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.basic {
  background: linear-gradient(90deg, rgba(131,58,180,1) 0%, rgba(253,29,29,1) 76%, rgba(252,176,69,1) 100%);;
}
.hot {
  background: radial-gradient(farthest-side at 10% 20%, rgb(255, 230, 0), rgb(245, 147, 0));
}

.cold {
  background: linear-gradient(40deg, rgba(0,9,36,1) 0%, rgba(9,38,121,1) 26%, rgba(0,212,255,1) 100%);;
}
</style>
