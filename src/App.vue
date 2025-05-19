<script setup>
import { ref, onMounted } from 'vue';
import KiraGranim from './components/granim.vue';
import ShareHeader from './components/ShareHeader.vue';
import TopContents from './components/TopContents.vue';
import PomodoloContents from './components/PomodoloContents.vue';

const page = ref('top')

function GotoPomodolo() {
  page.value = "pomodolo";
}
function GotoTop() {
  page.value = "top"
}

// 画面内の幅と高さ（必要に応じて動的取得もOK）
const WIDTH = ref(window.innerWidth);
const HEIGHT = ref(window.innerHeight);

const stars = ref([]);

function recreateStars() {
  const starinfo = {};
  starinfo['id'] = crypto.randomUUID();
  starinfo['x'] = Math.random() * WIDTH.value;
  starinfo['y'] = Math.random() * HEIGHT.value;
  starinfo['size'] = 5 + Math.random() * 15; // 5〜20pxくらいのサイズ
  starinfo['delay'] = 0;
  stars.value.push(starinfo)
}

// 星が消えるタイミングで削除（アニメ終了時）
function handleAnimationEnd(id) {
  stars.value = stars.value.filter(star => star.id !== id);
  recreateStars(); // 1つ消えたら、1つ補充
}

function handleResize() {
  WIDTH.value = window.innerWidth;
  HEIGHT.value = window.innerHeight;
}

onMounted(() => {
  let count = 0;
  const createstars = setInterval(()=>{
    if(count < 4){
      for (let i = 0; i < 3; i++) {
        recreateStars();
      };
      count += 1;
    }else{
      clearInterval(createstars)
    }
  },1000)
  window.addEventListener('resize', handleResize);

});

</script>
<template>
  <div>
    <ShareHeader class="header" @GotoTop="GotoTop" />
    <div class="contents">
      <TopContents v-if="page === 'top'" @GotoPomodolo="GotoPomodolo" />
      <PomodoloContents v-else-if="page === 'pomodolo'" />
      <svg :width="WIDTH" :height="HEIGHT" style="position: absolute; top: 0; left: 0; z-index: -1;">
        <g v-for="star in stars" :key="star.id" :transform="`translate(${star.x}, ${star.y})`" class="star"
          :style="{ animationDelay: star.delay + 's' }" @animationend="handleAnimationEnd(star.id)">
          <path
            d="M12 3L13.4302 8.31181C13.6047 8.96 13.692 9.28409 13.8642 9.54905C14.0166 9.78349 14.2165 9.98336 14.451 10.1358C14.7159 10.308 15.04 10.3953 15.6882 10.5698L21 12L15.6882 13.4302C15.04 13.6047 14.7159 13.692 14.451 13.8642C14.2165 14.0166 14.0166 14.2165 13.8642 14.451C13.692 14.7159 13.6047 15.04 13.4302 15.6882L12 21L10.5698 15.6882C10.3953 15.04 10.308 14.7159 10.1358 14.451C9.98336 14.2165 9.78349 14.0166 9.54905 13.8642C9.28409 13.692 8.96 13.6047 8.31181 13.4302L3 12L8.31181 10.5698C8.96 10.3953 9.28409 10.308 9.54905 10.1358C9.78349 9.98336 9.98336 9.78349 10.1358 9.54905C10.308 9.28409 10.3953 8.96 10.5698 8.31181L12 3Z"
            fill="white" :style="{ width: star.size + 'px', height: star.size + 'px' }" />
        </g>
      </svg>
      <KiraGranim />
    </div>
  </div>
</template>

<style scoped>
@keyframes twinkleMove {
  0%,100% {
    opacity: 0;
  }

  50% {
    opacity: 0.5;
  }
}

.star {
  animation: twinkleMove 4s linear 1;
  transform-origin: center;
}

.contents {
  width: 100%;
  box-sizing: border-box;
  margin: 16px auto;
  height: 100%;
  position: relative;
  overflow: hidden;
  /* はみ出し防止 */
}
</style>
