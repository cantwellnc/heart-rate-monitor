<template>
  <img alt="heart" src="./assets/heart2.gif">
  <heart-rate-display></heart-rate-display>
</template>

<script>
import HeartRateDisplay from './components/HeartRateDisplay.vue'

export default {
  name: 'App',
  components: {
    HeartRateDisplay
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  height: 100%;
}
img {
  border-radius: 10px;
  box-shadow: 2px 2px 5px #111111;
  margin-bottom: 20px;
}
</style>
