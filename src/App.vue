<script setup>
import {computed, onMounted, ref, unref} from "vue";
import axios from "axios";

const city = ref('')
const error = ref('')
const info = ref(null)

const displayCity = computed(() => {
  return city.value === '' ? 'вашем городе' : city.value
})

const showTemp = computed(() => {
  return `Температура: ${info.value && info.value.main ? info.value.main.temp : ''}`
})
const showFeelsLike = computed(() => {
  return `Ощущается как: ${info.value && info.value.main ? info.value.main.feels_like : ''}`
})
const showMinTemp = computed(() => {
  return `Минимальная температура: ${info.value && info.value.main ? info.value.main.temp_min : ''}`
})
const showMaxTemp = computed(() => {
  return `Максимальная температура: ${info.value && info.value.main ? info.value.main.temp_max : ''}`
})

const getWeather = () => {
  if(city.value.trim().length < 2) {
    error.value = 'Нужно название более одного символа'
    return false
  }

  error.value = ''

  getData()

}

const getData = async () => {
  try {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
      .then(res => (info.value = res.data))
  } catch (e) {
    console.log(e)
  }

}

</script>


<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ displayCity }}</p>
    <p v-if="error" style="color: red">{{ error }}</p>
    <input type="text" placeholder="Введите город" v-model="city"/>
    <button v-if="city" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите название города</button>

    <div v-if="info">
      <p>{{ showTemp }}1</p>
      <p>{{ showFeelsLike }}2</p>
      <p>{{ showMinTemp }}3</p>
      <p>{{ showMaxTemp }}4</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  color: white;
  padding: 20px;
  text-align: center;
}

.wrapper h1 {
  margin-top: 50px
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #9e2d7d;
}

.wrapper button:disabled {
  background: #7a6849;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3bc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translate(-5px);
}

</style>
