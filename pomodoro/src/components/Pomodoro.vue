<template>
  <div>
    <PomodoroTitle></PomodoroTitle>
    <PomodoroTomato></PomodoroTomato>
    <PomodoroButton v-on:play="ReEmitPlay" v-on:pause="ReEmitPause" v-on:stop="ReEmitStop"></PomodoroButton>
    <PomodoroTimer v-on:apiChange="ReEmitApiChange" v-on:reset="Reset" :timerState="this.TimerState"></PomodoroTimer>
  </div>
  
</template>

<script>
import PomodoroTitle from './PomodoroTitle'
import PomodoroTomato from './PomodoroTomato'
import PomodoroButton from './PomodoroButton'
import PomodoroTimer from './PomodoroTimer'
 
export default {
  name: 'Pomodoro',
  components: {
    PomodoroTitle,
    PomodoroTomato,
    PomodoroButton,
    PomodoroTimer,
  },
  methods: {
    //Re emit play event, originating in PomodoroButton
    ReEmitPlay() {
      this.$emit("play");
    },
    //Re emit pause event, originating in PomodoroButton
    ReEmitPause() {
      this.$emit("pause");
    },
    //Re emit stop event, originating in PomodoroButton
    ReEmitStop() {
      this.$emit("stop");
    },
    //Emit reset event
    Reset(){
      this.$emit("reset");
     },
    //Re emit play event, originating in PomodoroTimer
    ReEmitApiChange(){
      this.$emit('apiChange');
    }
  },
  data(){
    return{
      TimerState: false,
      PlayButtonState: true,
      PauseButtonState: false,
      StopButtonState: false
    }
  }
}
</script>

<style>

</style>