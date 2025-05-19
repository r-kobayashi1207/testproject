<script setup>
//ライブラリからのimport
import { ref,watch } from 'vue';

//PomodoloContents.vueから設定時間を受け取る
const props = defineProps({
  'worktime': String,
  'resttime': String,
})

//timerのタイプ切り替え（boolean）
const isWorking = ref(true);

//timerカウントの初期設定
let timerid = null;
const timecount = ref(0)

//円の初期設定
const cyclemax = 2 * Math.PI * 40;
const dashOffset = ref(0);

//作業時間の処理
const worktimecalc = props.worktime.split(':');
const settime = ref(Number(worktimecalc[0]) * 60 + Number(worktimecalc[1]));

//作業用タイマー
function starttimer() {
  if (timerid !== null) {
    return;
  }

  if(isWorking.value){
    timerid = setInterval(() => {
      if (settime.value === timecount.value) {
        clearInterval(timerid);
        timerid = null;
        timecount.value = 0;
        isWorking.value = false;
        dashOffset.value = 0;
        continuetimer();
        return;
      }
      timecount.value += 1;
      dashOffset.value = -(timecount.value / settime.value) * cyclemax;
    }, 1000)
  }else{
    continuetimer();
  }
}

//休憩時間の処理
const resttimecalc = props.resttime.split(':');
const takearest = ref(Number(resttimecalc[0]) * 60 + Number(resttimecalc[1]));


function continuetimer() {
  if (timerid !== null) {
    return;
  }
  if(!isWorking.value){
    timerid = setInterval(() => {
      if (takearest.value === timecount.value) {
        clearInterval(timerid);
        timerid = null;
        timecount.value = 0;
        isWorking.value = true;
        dashOffset.value = 0;
        starttimer();
        return;
      }
      timecount.value += 1;
      dashOffset.value = -(timecount.value / takearest.value) * cyclemax;
    }, 1000)
  }else{
    starttimer();
  }
}

function stoptimer() {
  clearInterval(timerid);
  timerid = null;
}

function resettimer() {
  clearInterval(timerid);
  timerid = null;
  timecount.value = 0;
  isWorking.value = true;
  dashOffset.value = 0;
}
defineExpose({ starttimer, stoptimer, resettimer })

// propの値の変化を検知して反映する
watch(() => props.worktime, (newVal) => {
  const [min, sec] = newVal.split(':').map(Number);
  settime.value = min * 60 + sec;
}, { immediate: true });
watch(() => props.resttime, (newVal) => {
  const [min, sec] = newVal.split(':').map(Number);
  takearest.value = min * 60 + sec;
}, { immediate: true });

</script>

<template>
  <div>

    <div class="pomodolo-timer" v-if="isWorking">

      <svg viewBox="0 0 100 100">
        <circle class="working" cx="50" cy="50" r="40" :stroke-dashoffset="dashOffset" :stroke-dasharray="cyclemax" />
      </svg>
      <p class="kiramoji">{{ String(Math.floor((settime - timecount) / 60)).padStart(2, '0') }}:{{
        String((settime - timecount) % 60).padStart(2, '0') }}</p>
      <img src="/mini1.png">

    </div>

    <div class="pomodolo-timer" v-else-if="!isWorking">
      <svg viewBox="0 0 100 100">
        <circle class="notworking" cx="50" cy="50" r="40" :stroke-dashoffset="dashOffset"
          :stroke-dasharray="cyclemax" />
      </svg>
      <p class="kiramoji">{{ String(Math.floor((takearest - timecount) / 60)).padStart(2, '0') }}:{{
        String((takearest - timecount) % 60).padStart(2, '0') }}</p>
      <img src="/mini2.png">
    </div>

  </div>


</template>

<style scoped>
.pomodolo-timer {
  position: relative;
  aspect-ratio: 1 / 1;
  height: 500px;
  margin: 16px;
}

svg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  overflow: visible;
  transform: rotate(-90deg);
}

/* サークルのCSS */
circle {
  fill: none;
  stroke-width: 10;
  stroke-linecap: round;
  transition: stroke-dashoffset 1s linear;
}

.working {
  stroke: rgb(250, 250, 210);
}

.notworking {
  stroke: rgb(221, 247, 249);
}

@keyframes girl-rolling {
  0% {
    transform: translate(-50%, -50%) rotate(0);
  }

  100% {
    transform: translate(-50%, -50%) rotate(360deg);
  }
}

img {
  position: absolute;
  width: 40%;
  height: auto;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) rotate(0deg);
  transform-origin: center center;
  animation: girl-rolling 60s linear infinite;
  z-index: 1;
}

p {
  position: absolute;
  width: 100%;
  text-align: center;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 5rem;
  z-index: 2;
  margin: 0;
}
</style>
