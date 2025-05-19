<template>
  <div>
    <canvas id="granim-canvas"></canvas>
  </div>
</template>

<script>
import Granim from 'granim';
export default {
  name: 'granim',
  data () {
    return {
      GObj: Object
    }
  },
  mounted () {
    this.GObj = new Granim({
      element: '#granim-canvas',
      name: 'granim',
      opacity: [1, 1],
      states: {
        'default-state': {
          gradients: [
             ['#faeae1', '#726294'],
             ['#1c1c30','#424254'],
             ['#0f0c17','#04010f'],
             ['#4b5f66','#996b74'],
             ['#82aacf','#d4c179'],
          ]
        }
      }
    })
  }
}
</script>

<style scoped>
#granim-canvas {
  top: 0; left: 0; right: 0; bottom: 0;
  width:100%;
  height:100%;
  position:fixed;
  z-index: -2;
}
</style>

