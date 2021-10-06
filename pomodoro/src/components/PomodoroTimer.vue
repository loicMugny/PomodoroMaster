<template>
  <div>
      <label ref="clock"></label><br/>
      <!-- Change the label state -->
      <label>{{ this.WorkingState ? "WORK!" : "REST!" }}</label>
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
        RestCount: 0,
        Minutes: 0,
        Seconds: -1,
        WorkMinutes: 25,
        WorkSeconds: 0,
        RestMinutes: 5,
        RestSeconds: 0,
        ResetMinutes: 25,
        ResetSeconds: 0
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
      // Set the rest timer and each 4 pomodori cycle change the rest
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
      'WorkTick': function(){
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
      // Emit event to change the cat each 5 seconds
      'RestTick': function(){
        if (!this.WorkingState && this.Active) {
          this.$emit('apiChange')
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
      this.WorkTimer = setInterval(this.WorkTick, 1000)
      this.RestTimer = setInterval(this.RestTick, 5000)
    }
}
</script>

<style>

</style>