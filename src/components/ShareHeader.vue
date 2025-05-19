<script setup>
const emit = defineEmits(['GotoTop'])
function GotoTop(){
  emit('GotoTop');
}

</script>

<template>
  <div>
    <p>DemoProject</p>
    <button @click = "GotoTop">Buck</button>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sacramento&display=swap');

div{
  background-color: white;
  position:relative;
}
p{
  margin:0;
  font-family: 'Sacramento', cursive;
  font-size: 32px;
  text-align: center;
}
button{
  font-family: 'Sacramento', cursive;
  position:absolute;
  top:50%;
  right:1rem;
  transform:translateY(-50%);
  z-index:2;
  border:none;
  transition: transform 1s ease;
}
button::after {
  background: #a4a4a4;
  position: absolute;
  top: 0;
  left: 0;
  content: '';
  width: 100%;
  height: 100%;
  transform: scale(0, 1);
  transform-origin: right top;
  transition: .2s cubic-bezier(0.45, 0, 0.55, 1);
  z-index: -1;
}
button:hover {
  color: #fff;
}
button:hover::after {
  transform: scale(1, 1);
}

</style>
