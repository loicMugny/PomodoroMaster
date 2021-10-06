<script src="https://kit.fontawesome.com/64c9512dc1.js" crossorigin="anonymous"></script>
<template>
  <div>
      <!--Pomodoro controls-->
    <button :disabled="isPlayDisabled" v-on:click="IsClicked('playButton')"><i class="fas fa-play"></i></button>
    <button :disabled="isPauseDisabled"  v-on:click="IsClicked('pauseButton')"><i class="fas fa-pause"></i></button>
    <button :disabled="isStopDisabled"  v-on:click="IsClicked('stopButton')"><i class="fas fa-stop"></i></button>
  </div>
</template>

<script>
export default {
    name: 'PomodoroButton',
    data(){
        return {
            isStopDisabled:false,
            isPauseDisabled:false,
            isPlayDisabled:false
        }
    },
    methods: {
        //Change button states according to which one was clicked and emit the proper event
        IsClicked(item) {
            switch(item) {
                default:
                case "playButton":
                    this.isPlayDisabled = true;
                    this.isPauseDisabled = false;
                    this.isStopDisabled = false;
                    this.$emit("play");
                    break;
                case "pauseButton":
                    this.isPlayDisabled = false;
                    this.isPauseDisabled = true;
                    this.isStopDisabled = false;
                    this.$emit("pause");
                    break;
                case "stopButton":
                    this.isPlayDisabled = false;
                    this.isPauseDisabled = false;
                    this.isStopDisabled=true;
                    this.$emit("stop");
                    break;
            }
        },
        //Reset buttons reset event is emitted from PomodoroTimer
        Reset() {
            this.isPlayDisabled = false;
            this.isPauseDisabled = false;
            this.isStopDisabled = false;
        }
    }
}
</script>

<style>

</style>