<script setup>
import TopClock from './ShareClock.vue';

const emit = defineEmits(['GotoPomodolo'])

function changetopomodolo() {
  // const audio = new Audio('click.mp3')
  // audio.play();
  emit('GotoPomodolo');
};

</script>

<template>
  <div>
    <img class="button" src="/kira.png" width="15%" @click="changetopomodolo" />
    <TopClock class="clock" />
    <img src="/girl.png" class="girl" />
  </div>
</template>

<style scoped>
@media screen and (min-width:1200px){
  .girl{
    width: 1200px !important;
    position:relative;
    left:50%;
    transform:translateX(-50%);
    box-sizing: border-box;
  }
}


.button {
  position: absolute;
  z-index: 1;
  right: 15%;
  top: 30%;
  transition: transform 0.5s ease;
}

.button:hover{
  transform:translateY(3px);
  filter: brightness(1.5);
}

.clock {
  position: absolute;
  font-size:5vw;
  z-index: 2;
  right: 10%;
  top: 10%;
}

.girl {
  width: 100%;
  box-sizing: border-box;
}
</style>
