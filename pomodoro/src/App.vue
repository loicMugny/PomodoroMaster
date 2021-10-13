<template>
  <div id="app">
    <pomodoro v-on:apiChange="ReEmitApiChange" class="component"></pomodoro>
    <cat-a-p-i class="component"></cat-a-p-i>
  </div>
</template>

<script>
import Pomodoro from './components/Pomodoro.vue'
import CatAPI from './components/CatApi.vue'

export default {
  name: 'app',
  components: {
    Pomodoro,
    CatAPI
  },
  methods:{
    // ReEmit the event to change the cat with the api
    'ReEmitApiChange': function(){
      this.$emit('apiChange');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  margin: auto;
  max-height: 100vh;
  width: 75%;
  padding: 0px;
}
</style>
