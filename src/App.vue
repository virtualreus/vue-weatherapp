<template>
<div className="wrapper">
  <h1>Погодное приложение </h1>
  <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName}}</p>

  <input type="text" v-model="city" placeholder="Введите город: ">
  <button v-if="city != ''" @click="getWeather()">Погода</button>
  <button disabled v-else>Введите название города</button>
  <p className="error">{{ error }}</p>

  <div v-if="info != null">
    <p>{{ showTemp }}</p>
    <p>{{ showFeelsLike }}</p>
    <p>{{ showMinTemp }}</p>
    <p>{{ showMinTemp }}</p>
  </div>
  
</div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return"\"" + this.city + "\""
    },
    showTemp() {
      return "Температура: " + this.info.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.temp_min
    },
    showMaxTemp() {
      return "Температура: " + this.info.temp_max
    },
  },
  methods: {
    getWeather() {
    if (this.city.trim().length < 2) {
      this.error = "Нужно >1 символа";
      return false;
    }
    this.error = "";
    
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=535f4db4f3c08f43f2edf97c31655682`;
    axios.get(url).then(res => (this.info = res.data.main));
    console.log(this.info);
  }

  }
  
}
</script>


<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: #1f0f24;
  text-align: center;
  color: #fff;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
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
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: 0.15s;
}

.wrapper button {
  background-color: #e3bc3b;
  color: white;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  cursor: pointer;
  margin-left: 20px;
  transition: transform 200ms ease;
}

.wrapper button:hover {
  transform: scale(1.1);
}

.wrapper button:disabled {
  background-color: #c4a549;

}

.wrapper input:focus,  .wrapper input:hover{
  border-bottom-color: #6e2d7d;
}
</style>
