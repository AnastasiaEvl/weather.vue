<script>
import axios from 'axios'
export default {
  data(){
    return {
      city: '',
      error:'',
      info: null
    }
  },
  computed: {
    cityName(){
      return '<' + this.city + '>'
    },
    showTemp(){
      return "Temperature: " + (this.info.main.temp).toFixed(1) + " °C"
    },
    showFeelsLike(){
      return "Feels like: " + (this.info.main.feels_like).toFixed(1) + " °C"
    },
    showMinTemp(){
      return "Min temperature: " + (this.info.main.temp_min).toFixed(1) + " °C"
    },
    showMaxTemp(){
      return "Max temperature: " + (this.info.main.temp_max).toFixed(1) + " °C"
    }
  },
  methods: {
    getWeather(){
      if(this.city.trim().length <2){
        this.error='Enter correct city name'
        return false
      }
      this.error=''
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f4fe30cdc2392172ce0d8e8bc6ce4dde`)
          .then(res=> (this.info = res.data))
    }
  }
}

</script>

<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>Know weather in {{ city !== '' ? cityName : 'your city' }}</p>
    <input type="text" v-model="city" placeholder="Enter your city">
    <button v-if="city !== ''" @click="getWeather()">Get Weather</button>
    <button disabled v-else>Enter city</button>
    <p class="error">{{error}}</p>
    <div v-if="info !== null">
      <p >{{showTemp}}</p>
      <p >{{showFeelsLike}}</p>
      <p >{{showMaxTemp}}</p>
      <p >{{showMinTemp}}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: red;
}
.wrapper {
  width: 100%;
  height: 500px;
  border-radius: 50px;
  background: #080643;
  margin: 5rem auto;
  padding: 20px;
  text-align: center;
  color: white;
}

.wrapper h1 {
  margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border:0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
}
.wrapper input:focus {
  border-bottom-color: #2c3e50;
}
.wrapper button {
  background: #e3bc4b;
  color:#fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}
.wrapper button:hover {
  transform:scale(1.1)
}
.wrapper button:disabled {
  background-color: #9b8b5d;
  cursor: not-allowed;
}
</style>
