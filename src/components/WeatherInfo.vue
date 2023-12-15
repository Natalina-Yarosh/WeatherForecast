<script>
export default {
  props:{
        currentDate: {
            type: String,
            required: true
        },
        icon: {
            type: String,
            required: true
        },
        info: {
            type: String,
            required: true
        },
        temperatureMin: {
            type: String,
            required: true
        },
        temperatureMax: {
            type: String,
            required: true
        },
    }
}
</script>

<template>
<div class="wrap__info">
    <p class="day">{{ currentDate }}</p>
    <div class="content" :style="{ 'background-image': `url(${icon})` }">
        <p class="content__info">{{ info }}</p>
        <p class="content__degree">{{ temperatureMin }}° / {{ temperatureMax }}°</p>
    </div>
</div>
</template>

<style scoped>
.wrap__info {
    padding: 20px;
    background: rgb(215, 215, 215);
}
.day {
    font-size: 20px;
    font-weight: 600;
    line-height: 25px;
    margin-bottom: 10px;
    color: var( --primary-color);
}
.content {
    background-size: 40px;
    background-position: right;
    background-repeat: no-repeat;
    padding-right: 50px;
}
.content__info {
    font-size: 16px;
    line-height: 18px;
    margin-bottom: 3px;
    color: var(--secondary-color);
    text-transform: capitalize;
}
.content__degree{
    font-size: 16px;
    line-height: 18px;
    color: var(--secondary-color);
}
</style>