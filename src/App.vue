<script setup>
import { onMounted } from 'vue';
const el = $ref()
const ctx = $computed(() => el.getContext('2d'))
function init() {
  ctx.strokeStyle = '#d3d3d3'
  ctx.moveTo(100,100)
  ctx.lineTo(200,200)
  ctx.stroke()
}


onMounted(() => {
  init()
  
})
</script>

<template>
  <canvas ref="el" width="600" height="600" style="border:1px solid #d3d3d3;"></canvas>
</template>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
