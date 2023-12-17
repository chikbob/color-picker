<template>
  <div class="colors">
    <color/>
    <color/>
    <color/>
    <color/>
    <color/>
  </div>
</template>

<script setup>
import color from "./color__display.vue";
</script>

<style lang="scss" scoped>
.colors {
  display: flex;
  flex-direction: row;
}
</style>