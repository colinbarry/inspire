<script setup>
import { onMounted, ref } from "vue"

const State = {
  INHALE: 0,
  HOLD_IN: 1,
  EXHALE: 2,
  HOLD_OUT: 3,
}

const state = ref()

const nextState = () => {
  state.value = (state.value + 1) % 4
}

onMounted(() => {
  requestAnimationFrame(() =>
    requestAnimationFrame(() => {
      state.value = State.INHALE
      setInterval(nextState, 4000)
    }),
  )
})
</script>

<template>
  <div class="outer">
    <div class="canvas">
      <div class="inner" />
      <div
        class="inner hold-in animation"
        :class="{ 'hold-in-animation': state == State.HOLD_IN }"
      >
        hold
      </div>
      <div class="inner" />
      <div
        class="inner inhale animation"
        :class="{ 'inhale-animation': state == State.INHALE }"
      >
        inhale
      </div>
      <div class="inner box" />
      <div
        class="inner exhale animation"
        :class="{ 'exhale-animation': state == State.EXHALE }"
      >
        exhale
      </div>
      <div class="inner" />
      <div
        class="inner hold-out animation"
        :class="{ 'hold-out-animation': state == State.HOLD_OUT }"
      >
        hold
      </div>
      <div class="inner" />
    </div>
  </div>
</template>

<style scoped>
.outer {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.canvas {
  aspect-ratio: 1;
  display: grid;
  height: 100%;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: 1fr 1fr 1fr;
  margin: 0 auto;
}

.inner {
  margin: 3%;
  font-size: 150%;
  font-weight: 700;
}

.animation {
  animation-duration: 4000ms;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in-out;
}

.inhale-animation {
  animation-name: inhale;
}
.exhale-animation {
  animation-name: exhale;
}
.hold-in-animation {
  animation-name: hold-in;
}
.hold-out-animation {
  animation-name: hold-out;
}

.inhale {
  display: flex;
  justify-content: right;
  align-items: center;
  opacity: 0;
}

.exhale {
  display: flex;
  justify-content: left;
  align-items: center;
  opacity: 0;
}

.hold-in {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  opacity: 0;
}
.hold-out {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  opacity: 0;
}

@keyframes inhale {
  0% {
    transform: translateY(50%);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    transform: translateY(-50%);
    opacity: 0;
  }
}

@keyframes exhale {
  0% {
    transform: translateY(-50%);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    transform: translateY(50%);
    opacity: 0;
  }
}

@keyframes hold-in {
  0% {
    transform: translateX(-50%);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateX(50%);
    opacity: 0;
  }
}

@keyframes hold-out {
  0% {
    transform: translateX(50%);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    transform: translateX(-50%);
    opacity: 0;
  }
}

.box {
  border: 2px solid white;
  border-radius: 5%;
}
</style>
