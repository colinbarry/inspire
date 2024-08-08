<script setup>
import { ref, onMounted } from "vue"

const blobs = ref([])

const rnd = () => Math.random()

for (let i = 0; i < 80; ++i) {
  const blob = {
    index: i,
    size: rnd() * 0.5,
    x: rnd(),
    y: rnd(),
    xinc: rnd() * 0.001,
    yinc: rnd() * 0.001,
  }
  blobs.value.push(blob)
}

onMounted(() => {
  const frame = () => {
    for (let blob of blobs.value) {
      blob.x += blob.xinc
      blob.y += blob.yinc

      if (blob.x > 1.5) {
        blob.x = -0.5
      }
      if (blob.y > 2.5) {
        blob.y = -0.5
      }
    }
    requestAnimationFrame(frame)
  }
  requestAnimationFrame(frame)
})
</script>

<template>
  <svg class="background" viewBox="0 0 1 1" preserveAspectRadio="cover">
    <circle
      v-for="blob in blobs"
      :key="blob.index"
      class="node"
      :r="blob.size"
      fill="#f64f59"
      :style="{
        width: `${blob.size * 30}%`,
        height: `${blob.size * 30}%`,
        transform: `translate(${blob.x * 200}%, ${blob.y * 200}%)`,
      }"
    ></circle>
  </svg>
</template>

<style scoped>
.background {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  filter: blur(16px);
}
</style>
