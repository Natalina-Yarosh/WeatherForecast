<template>
   <div>
        <p v-if="isLoading" class="loading"></p>
        <div v-else class="item">
            <CityImage :cityImage="cityImage" :className="className"   :temperature="temperature" :city="city"/>
            <WeatherInfo
            :city="city"
            :temperature="temperature"
            :info="info"
            :temperatureMin="temperatureMin"
            :temperatureMax="temperatureMax"
            :currentDate="currentDate"
            :icon="icon"
            />
        </div>
   </div>
</template>

<script>
import dayjs from 'dayjs';
import axios from 'axios';
import { openWeatherMapApiUrl, openImageApiUrl } from '../assets/api';

import CityImage from './CityImage.vue';
import WeatherInfo from './WeatherInfo.vue';

export default {
    components: {
        CityImage,
        WeatherInfo,
    },
    data(){
      return{
        temperature: '',
        temperatureMin: '',
        temperatureMax: '',
        city: '',
        date:'',
        info:'',
        degrees: '',
        currentDate: dayjs().locale('en').format('dddd, D MMMM'),
        className:'',
        icon:'',
        cityImage: '',
        isLoading: false,
      }
    },
    mounted(){
        this.getLocation();
        this.fetchRandomCityImage();
    },
    methods:{
        getLocation() {
            this.isLoading = true;
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition((position) => {
                this.showWeather(position); 
                });
            } else {
                alert("Geolocation is not supported by your browser.");
            }
        },
        async showWeather(position) {
            const apiKey = '3d51f680ac485452eada2b919d5a39bd';
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;
            const weatherApiUrl = `${openWeatherMapApiUrl}?lat=${latitude}&lon=${longitude}&appid=${apiKey}`;


            axios
                .get(weatherApiUrl)
                .then(response => {
                    this.isLoading = false;
                    const data = response.data;
                    this.city = data.name;
                    this.temperature = (data.main.temp - 273.15).toFixed(1);
                    this.info = data.weather[0].description;
                    this.temperatureMin = (data.main.temp_min - 273.15).toFixed(1);
                    this.temperatureMax = (data.main.temp_max - 273.15).toFixed(1);
                    const iconCode = data.weather[0].icon;
                    this.icon = `https://openweathermap.org/img/wn/${iconCode}.png`;

                    if (this.temperature < -10) {
                        this.className = 'blue';
                    } else if (this.temperature > -10 && this.temperature <= 10) {
                        this.className = 'light-blue';
                    } else if (this.temperature > 10 && this.temperature <= 25) {
                        this.className = 'yellow';
                    } else {
                        this.className = 'red';
                    }
                })
                .catch(error => console.error('Error during API request:', error));
        },
        async fetchRandomCityImage() {
            const imageApiUrl = `${openImageApiUrl}`;
            try {
                const response = await axios.get(imageApiUrl, {
                    params: {
                        query: 'city',
                        orientation: 'landscape',
                        client_id: 'ofMQDoT-icgdK_vQb4PitqCNYTa5KL-msxb5BrnHjr8',
                    },
                });
                this.cityImage = response.data.urls.regular;
            } catch (error) {
                console.error('Error when uploading an image:', error);
            }
        },
    },
}
</script>

<style scoped>
.item {
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0px 9px 19px 0px #121620;
    min-width: 300px;
}

@keyframes animation{
    0%{
        transform: rotate(0deg);
    }
    100%{
        transform: rotate(360deg);
    }
}

.loading[data-v-e0cda9ff] {
    padding: 30px;
    width: 50px;
    height: 50px;
    margin: 100px auto;
    border-radius: 50%;
    border: 5px solid #514d4d;
    position: relative;
    border-top: 5px solid white;
    animation: animation .5s linear infinite;
}
</style>