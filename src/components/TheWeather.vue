<template>
   <div class="item">
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
</template>

<script>
import dayjs from 'dayjs';
import axios from 'axios';

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
        cityImage: ''
      }
    },
    mounted(){
        this.getLocation();
        this.fetchRandomCityImage();
    },
    methods:{
        getLocation() {
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
            const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}`;

            axios
                .get(apiUrl)
                .then(response => {
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
            try {
                const response = await axios.get('https://api.unsplash.com/photos/random', {
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
</style>