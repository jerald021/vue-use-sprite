<template>
  <div class="sprite" :style="`background-position: ${activePosition}px 50%;`"></div>
  <input type="range" step="1" min="1" max="20" v-model="speed" />
  <p>This is current speed: {{speed}}</p>

  <button @click="isActive ? pause() : resume()">
    {{isActive ? 'Pause' : 'Resume'}}
  </button>

</template>

<script setup>
// import { useIntervalFn } from '@vueuse/core';
import { ref } from 'vue';
import { useRafFn } from '@vueuse/core';

const activePosition = ref(0);
const speed = ref(10);
const framesComplete = ref(0);

const {pause, resume, isActive} = useRafFn(() => {
  framesComplete.value++;
  if(framesComplete.value % speed.value) return
  if (activePosition.value > -525) {
    activePosition.value -= 75;
  } else {
    activePosition.value = 0;
  }
});
</script>

<style>
input[type='range'] {
  transform: scaleX(-1);
}
.sprite{
  /* display the image*/
  background: url(https://freesvg.org/img/1525205509.png) no-repeat;

  /* each frame is 75px wide so limit container to display one at a time */
  width: 75px; 

  /* main is roughly 150px tall */
  height: 150px;

  /* the image has some space on top and bottom so this accounts for that */
  background-position: 0px 50%;
  
}
</style>
