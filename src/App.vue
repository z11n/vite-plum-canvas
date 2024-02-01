<script setup>
import { onMounted } from "vue";
const WIDTH = 600;
const HEIGHT = 600;
const el = $ref();
const ctx = $computed(() => el.getContext("2d"));
let pendingTasks = [];

function init() {
  drawLine({ start: { x: 300, y: 600 }, length: 10, theta: -Math.PI / 2 });
}

function drawLine(branch, depth = 0) {
  if (depth > 30) return;
  const end = getEndPoint(branch);
  lineTo(branch.start, end);
  if (depth < 7 || Math.random() < 0.5) {
    pendingTasks.push(() => {
      drawLine(
        {
          start: end,
          length: branch.length + Math.random() * 2,
          theta: branch.theta - 0.3 * Math.random(),
        },
        depth + 1
      );
    });
  }

  if (depth < 2 || Math.random() < 0.5) {
    pendingTasks.push(() => {
      drawLine(
        {
          start: end,
          length: branch.length + Math.random() * 2,
          theta: branch.theta + 0.3 * Math.random(),
        },
        depth + 1
      );
    });
  }
}

function lineTo(p1, p2) {
  requestAnimationFrame(() => {
    ctx.beginPath();
    ctx.fillStyle = "#eee";
    ctx.moveTo(p1.x, p1.y);
    ctx.lineTo(p2.x, p2.y);
    ctx.stroke();
  });
}

function getEndPoint(b) {
  return {
    x: b.start.x + b.length * Math.cos(b.theta),
    y: b.start.y + b.length * Math.sin(b.theta),
  };
}

function frame() {
  const tasks = [];
  pendingTasks = pendingTasks.filter((i) => {
    if (Math.random() > 0.4) {
      tasks.push(i);
      return false;
    }
    return true;
  });
  tasks.forEach((fn) => fn());
}

let framesCount = 0;
function startFrame() {
  requestAnimationFrame(() => {
    framesCount += 1;
    if (framesCount % 3 === 0) frame();

    startFrame();
  });
}

startFrame();

onMounted(() => {
  init();
});
</script>

<template>
  <canvas
    ref="el"
    width="600"
    height="600"
    style="border: 1px solid #d3d3d3"
  ></canvas>
</template>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
