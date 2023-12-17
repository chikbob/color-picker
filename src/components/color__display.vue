<template>
  <div @click="activate" class="color" :style="{backgroundColor: rgb }">
    <div @click="copyToClick(rgb)" class="color__hex" :style="{ color: text_rgb }">{{ rgb }}</div>
    <button @click="lockColorActivator"
            v-if="lock === false"
            class="color__button fa-solid fa-lock-open"
            :style="{ color: text_rgb }"></button>
    <button @click="lockColorActivator" v-else class="color__button fa-solid fa-lock"
            :style="{ color: text_rgb }"></button>
  </div>
</template>

<script setup>
import {ref, onBeforeMount} from "vue";

let rgb = ref('')
let text_rgb = ref('')
let checker = true
let lock = ref(false)
let copy = false

function copyToClick(text) {
  navigator.clipboard.writeText(text)
  copy = true
  return deActivate()
}

function deActivate() {
  setTimeout(function () {
    copy = false
    checker = true
  }, 200);
}

function activate() {
  checker = false
  setRandomColors()
  deActivate()
}

document.addEventListener('keydown', (event) => {
  if (event.code.toLowerCase() === 'space' && checker === true && lock.value === false) {
    copy = false
    activate()
  }
})

function lockColorActivator() {
  const localLock = !lock.value
  copy = true
  return lock.value = localLock
}

function setRandomColors() {
  if (lock.value === false && copy === false) {
    let newRGB = ``
    const randomBetween = (min, max) => min + Math.floor(Math.random() * (max - min + 1));
    const r = randomBetween(0, 255)
    const g = randomBetween(0, 255)
    const b = randomBetween(0, 255)
    const sum = r + g + b

    if (r >= 165 && g >= 165 || g >= 165 && b >= 165 || b >= 165 && r >= 165 || r >= 200 && sum >= 350 || g >= 200 && sum >= 350 || r >= 230 || g >= 215) {
      text_rgb.value = 'rgb(30, 30, 30)'
    } else {
      text_rgb.value = 'rgb(240, 240, 240)'
    }
    newRGB = `rgb(${r},${g},${b})`
    rgb.value = newRGB
    return rgb.value
  }
  return 0
}

onBeforeMount(() => {
  setRandomColors()
})


</script>

<style lang="scss" scoped>
.color {
  height: 100vh;

  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;

  border: 0px;

  &__hex {
    user-select: none;

    margin: 0;
    padding: 0.7rem;
    border-radius: 1rem;
    transition: background 0.3s;

    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    font-size: 1.2rem;
    font-weight: 500;

    &:hover {
      background: rgba(0, 0, 0, 0.1);
      cursor: pointer;
    }
  }

  &__button {
    outline: none;
    border: 0;
    background: transparent;
    font-size: 1.5rem;
    padding: 0.7rem;
    border-radius: 1rem;
    transition: background 0.3s;

    &:hover {
      background: rgba(0, 0, 0, 0.1);
      cursor: pointer;
    }
  }
}
</style>