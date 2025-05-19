<script setup>
import { ref, onMounted } from 'vue';
const nowtime = ref(new Date(Date.now()));

onMounted(() => {
  setInterval(() => {
    nowtime.value = new Date(Date.now())
  }, 1000)
})

const daylist = ['SUN','MON','TUE', 'WED', 'THU', 'FRI'];

</script>
<template>
  <div>
    <p class="date">{{ `${String(nowtime.getMonth() + 1).padStart(2,'0')}-${String(nowtime.getMonth() + 1).padStart(2,'0')} ${daylist[nowtime.getDay()]}` }}</p>
    <p class="time">{{ `${String(nowtime.getHours()).padStart(2,'0')}:${String(nowtime.getMinutes()).padStart(2,'0')}:${String(nowtime.getSeconds()).padStart(2,'0')}` }}</p>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Itim&display=swap');

p {
  font-family: 'Share Tech Mono', monospace;
  margin: 0;
  text-align: center;
  color: #e9feff;
    text-shadow: 0 0 20px #c2f2ff;
}
.time{
  font-size:5vw;
}
.date{
  font-size:4vw;
}
</style>
