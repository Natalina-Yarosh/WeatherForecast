<template>
    <div class="loading"></div>
</template>

<style scoped>
@keyframes animation{
    0%{
        transform: rotate(0deg);
    }
    100%{
        transform: rotate(360deg);
    }
}

.loading {
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