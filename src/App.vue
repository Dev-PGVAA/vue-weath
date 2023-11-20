<script>
import axios from 'axios'

export default {            
  data(){
    return {
      city: "",
      er: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "городе «" + this.city + "»"
    },
    
    ShowTemp(){
      return "Температура: " + this.info.main.temp + "°C"
    },
    ShowFeelsLike(){
      return "Ощущается как: " + this.info.main.feels_like + "°C"
    },
    ShowMinTemp(){
      return "Минимальная t на 1 час: " + this.info.main.temp_min + "°C"
    },
    ShowMaxTemp(){
      return "Максимальная t на 1 час: " + this.info.main.temp_max + "°C"
    },
    ShowWind(){
      return "Скорость ветра: " + this.info.wind.speed + "м/с"
    },

  },
  methods: {
    GetWeather(){
      if(this.city.trim().length < 2) {
        this.er = "Введите город"
        return false
      }

      this.er = ""
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e387eb10ceac91e12aeb38f5ee5670e9`)
      .then(res => (this.info = res.data))
      .catch(err=>(this.er = "Город не найден"))
    },
    ChangeI(){
      this.info = null;
      this.er = "";
    }
  }   
}
</script>

<template>
  <div class='wrapper'>
    <div class='caption-wrapper'>
      <h1>Погодное приложение</h1>
      <h3 style='margin-top: 1vh'>Узнать погоду в <span class='textCity'>{{ city == "" ? "вашем городе" : cityName }}</span></h3>
    </div>
    <div class='dialog-wrapper'>
      <input type="text" v-model='city' v-on:input='ChangeI()' v-on:keyup.enter='GetWeather()' class='I-city' placeholder='Введите город: '>
      <button v-show="city != ''" @click="GetWeather()" id='B-GetWeather'> Узнать </button>
    </div>
    <p class='error' v-if='er != ""'>{{ er }}</p>
    <div id='info' v-if="info != null">
      <p>{{ ShowTemp }}</p>
      <p>{{ ShowMaxTemp }}</p>
      <p>{{ ShowMinTemp }}</p>
      <p>{{ ShowFeelsLike }}</p> 
      <p>{{ ShowWind }}</p>
    </div>
    
  </div>
</template>

<style scoped>
</style>