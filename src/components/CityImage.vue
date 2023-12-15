<script>
export default {
  props:{
        className: {
            type: String,
            required: true
        },
        cityImage: {
            type: String,
            required: true
        },
        temperature: {
            type: String,
            required: true
        },
        city: {
            type: String,
            required: true
        },
    }
}
</script>

<template>
    <div :class="[className, 'wrap__image']" :style="{ 'background-image': `url(${cityImage})` }">
        <p class="degree">{{ temperature }}°</p>
        <p class="name">{{ city }}</p>
    </div>
</template>

<style scoped>
.wrap__image {
    aspect-ratio: 1 / 1.2;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    padding: 20px;
    position: relative;
}
.wrap__image::before{
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: transparent;
}
.blue::before{
    background: rgba(14, 2, 47, 0.7);
}
.light-blue::before{
    background: rgba(53, 10, 170, 0.7);
}
.yellow::before{
    background: rgba(188, 188, 4, 0.7);
}
.red::before{
    background: rgba(100, 14, 5, 0.7);
}
.name {
    font-size: 16px;
    line-height: 18px;
    color: var(--white-color);
    position: relative;
    z-index: 1;
    text-transform: uppercase;
}
.degree {
    font-size: 30px;
    line-height: 40px;
    margin-bottom: 10px;
    color: var(--white-color);
    position: relative;
    z-index: 1;
}
</style>