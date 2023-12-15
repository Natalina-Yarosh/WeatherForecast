<template>
  <div class="wrapper">
      <TheWeather/>
  </div>
</template>

<script>
import TheWeather from './components/TheWeather.vue'
export default {
  components: {TheWeather},
}
</script>

<style scoped>
.wrapper{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    
}
</style>
