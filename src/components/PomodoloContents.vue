<script setup>
import TimerCycle from './TimerCycle.vue'
import { ref, watch } from 'vue';

const [worktime, resettime] = [ref('00:10'), ref('00:20')];

const [defaultwork, defaultrest] = [ref(worktime.value), ref(resettime.value)]

const timerRefs = ref(null);

function settime() {
  if (worktime.value === '00:00' || resettime.value === '00:00') {
    return;
  }
  if (!confirm("設置時間を変更します。タイマーをリセットしても良いですか？")) {
    return;
  }
  change.value = false;
  defaultwork.value = worktime.value;
  defaultrest.value = resettime.value;
  parentreset();
}

function parentstart() {
  timerRefs.value?.starttimer();
}
function parentstop() {
  timerRefs.value?.stoptimer();
}
function parentreset() {
  timerRefs.value?.resettimer();
}

const change = ref(false);

watch([worktime, resettime], () => {
  change.value = true;
})

</script>

<template>
  <div class="pomodolocontainer">
    <TimerCycle class="timer" :worktime="defaultwork" :resttime="defaultrest" ref="timerRefs" />

    <div class="buttons">
      <button @click="parentstart">
        <p class="kiramoji">Start</p>
      </button>
      <button @click="parentstop">
        <p class="kiramoji">Stop</p>
      </button>
      <button @click="parentreset">
        <p class="kiramoji">Reset</p>
      </button>
    </div>

    <form @submit.prevent="settime">
      <label>
        <p class="settime">作業時間</p>
        <input type="time" v-model="worktime">
      </label>
      <label>
        <p class="settime">休憩時間</p>
        <input type="time" v-model="resettime">
      </label>
      <svg v-if="change" class="changekey" viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg">
        <path d="M8 6L8 2L10 2L16 8L10 14L8 14L8 10L0 10L0 6L8 6Z" fill="currentColor" />
      </svg>
      <button class="setbutton">
        <p class="kiramoji">Set</p>
      </button>
    </form>

  </div>
</template>

<style scoped>
@keyframes arrowmove {
  0% {
    transform: translateX(-10%);
    color: rgba(255, 255, 255, 0);
  }

  100% {
    transform: translateX(10%);
  }
}

.changekey {
  width: 30px;
  position: relative;
  color: rgba(255, 255, 255, 0.5);
  animation: arrowmove 1.3s infinite;
}

input {
  all: unset;
  box-sizing: border-box;

}

form {
  display: flex;
  gap: 16px;
  align-items: center;
}

label {
  padding: 4px;
  border-radius: 4px;
  background-color: rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
}

.settime {
  margin: 0;
  padding: 0;
  display: inline-block;
  text-align: center;
  font-family: 'Share Tech Mono', monospace;
  font-size: 12px;
  color: rgb(68, 68, 68);
}

/* .setbutton{
  margin-left:32px;
} */
.pomodolocontainer {
  display: flex;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  flex-direction: column;
  align-items: center;
}

.buttons {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

button {
  all: unset;
  padding: 4px 8px;
  border-radius: 5%;
  transition: background-color 0.1s;
}

button p {
  margin: 0;
  padding: 0;
  text-align: center;
}

button:hover {
  transform: scale(1.2);
  background-color: rgba(255, 255, 255, 0.3);
}

.timer {
  flex: 1;
  height: auto;
}
</style>
