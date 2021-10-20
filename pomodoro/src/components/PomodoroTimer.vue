<template>
  <div>
      <label ref="clock"></label><br/>
      <!-- Change the label state -->
      <label>{{WorkingState ? "WORK!" : "REST!" }}</label>
  </div>
</template>

<script>
export default {
    name: 'PomodoroTimer',
    data(){
      return {
        WorkingState: false,
        Active: this.timerState,
        PomodoriCycle: 4,
        RestCount: 1,
        Minutes: 0,
        Seconds: -1,
        WorkMinutes: 20,
        WorkSeconds: 0,
        RestMinutes: 20,
        RestSeconds: 0,
        ResetMinutes: 20,
        ResetSeconds: 0,
        RestTickCount: 0
      }
    },
    methods: {
      // Play the timer
      'play': function(){
          this.Active = true
          if(this.Seconds == -1){
            this.work()
          }
      },
      // Pause the timer
      'pause': function(){
          this.Active = false
      },
      // Set the work timer
      'work': function() {
        this.Minutes = this.WorkMinutes
        this.Seconds = this.WorkSeconds
        this.WorkingState = true
      },
      // Set the rest timer and each n pomodori cycle change the rest
      'rest': function(){
        if(this.RestCount != this.PomodoriCycle) {
          this.RestCount++
          this.Minutes = this.RestMinutes
          this.Seconds = this.RestSeconds
        } else {
          this.RestCount = 0
          this.Minutes = this.RestMinutes * 3
          this.Seconds = this.RestSeconds
        }
        this.WorkingState = false
      },
      // Reset the timer
      'reset': function(){
        this.Minutes = this.ResetMinutes
        this.Seconds = this.ResetSeconds
        this.$emit('reset')
      },
      // Decrease the pomodori and change the state
      'workTick': function(){
        if(this.Active && this.WorkingState){
          if(this.Seconds == 0 && this.Seconds != null){
            this.Minutes--
            this.Seconds = 59
          }else{
            this.Seconds--
          }
          if (this.Minutes == 0 && this.Seconds == 0){
            if(this.WorkingState){
              this.rest()
            }else{
              this.work()
            }
          }
          // Display the timer
          this.$refs.clock.innerHTML = this.Minutes + ":" + (this.Seconds > 9 ? this.Seconds : "0" + this.Seconds)
        }
      },
      // Decrease the pomodoro and change the state then emit event to change the cat each 5 seconds
      'restTick': function(){

          if(this.RestTickCount == 4){
            this.RestTickCount = 0
            this.$emit('apiChange')
          }

        if (!this.WorkingState && this.Active) {
          if(this.Seconds == 0 && this.Seconds != null){
            this.Minutes--
            this.Seconds = 59
          }else{
            this.Seconds--
          }
          if (this.Minutes == 0 && this.Seconds == 0){
            if(this.WorkingState){
              this.rest()
            }else{
              this.work()
            }
          }
          this.RestTickCount++

          // Display the timer
          this.$refs.clock.innerHTML = this.Minutes + ":" + (this.Seconds > 9 ? this.Seconds : "0" + this.Seconds)
        }
      }
    },
    props: {
      timerState: Boolean,
    },
    // Get the parent event and start the timer
    created(){
      this.$parent.$on('play', this.play)
      this.$parent.$on('pause', this.pause)
      this.$parent.$on('stop', this.reset)
      this.WorkTimer = setInterval(this.workTick, 1000)
      this.RestTimer = setInterval(this.restTick, 1000)
    }
  }

</script>

<style>

</style>
