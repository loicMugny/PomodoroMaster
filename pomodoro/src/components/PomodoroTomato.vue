<template>
  <div>
    <img id="img" class="tomato" alt="Tomato" src="../assets/tomato.png">
  </div>
</template>

<style>

div{
  margin : auto;
}

.tomato{
  width: auto;
  height: 45vh;
  animation: grayInLoad 2s infinite;
  animation-direction: alternate-reverse;
}
@keyframes grayInLoad {
  from {
    filter: grayscale(0%);
  }
  to {
    filter: grayscale(100%);
  }
}
</style>