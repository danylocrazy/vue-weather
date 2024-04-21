<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>weather in  {{ city == "" ? " your city" : cityName }}</p>
    <input type="text" v-model="this.city" 
    placeholder="enter name your city">

    <button v-if="city != ''"  @click="getWeather()">Get weather</button>
    <button disabled v-else="city != ''">Enter name city</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null">
    <p>{{ showTemp }}</p>
    <p>{{ showFeelsLike }}</p>
    <p>{{ showMaxTemp }}</p>
    <p>{{ showMinTemp }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      city: "",
      error: "",
      info: null,
      }
    },
    computed: {
      cityName() {
        return "'" + this.city + "'"; 
      },
      showTemp() {
        return "temperature: " + this.info.main.temp 
      },
      showFeelsLike() {
        return "feelslike: " + this.info.main.feels_like
      },
      showMaxTemp() {
        return "max temperature: " + this.info.main.temp_max
      },
      showMinTemp() {
        return "min temperature: " + this.info.main.temp_min
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "Enter correct name"; 
          return false
        } 
          this.error = "";
          axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
            .then(res => (this.info = res.data))
      }
    }

  }
</script>

<style scoped>

  .error {
    color: red;
  }

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background-color: rgb(1, 4, 26);
    padding: 20px;
    text-align: center;
    color: #fff;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 20px;
  }

  .wrapper input {
    margin-top: 30px;
    background-color: transparent;
    border: 0;
    border-bottom: 1px solid black;
    color: #fff;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    }

    .wrapper input:focus {
      border-bottom-color: #6e2d7d;
    }

    .wrapper button:disabled { 
      background-color: #ad891c;
      cursor: not-allowed;
    }

    .wrapper button {
      background-color: #e3bc4b;
      color: #fff;
      border-radius: 10px;
      border: 2px solid #b99935;
      padding: 10px 15px;
      margin-left: 20px;
      cursor: pointer;
      transition: transform 500ms ease;
    }

    .wrapper button:hover {
      transform: scale(1.1) translateY(-5px);
    }
</style>