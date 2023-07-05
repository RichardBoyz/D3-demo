<template>
  <div ref="chartContainer"></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import * as d3 from "d3";

const chartContainer = ref(null);
let intervalId;
const dataset = Array.from({ length: 10 }, () => ({ x: 0, y: 0 })); // 固定資料集長度為10，初始值為 { x: 0, y: 0 }

// 生成隨機數值
function generateRandomValue() {
  return Math.random() * 100; // 可自行調整數值範圍
}

onMounted(() => {
  const width = 400;
  const height = 300;

  const xScale = d3.scaleLinear().range([0, width]);
  const yScale = d3.scaleLinear().range([height, 0]);

  const line = d3
    .line()
    .x((d, i) => xScale(i)) // 使用索引作為 x 軸座標
    .y((d) => yScale(d.y)); // 使用 yScale 設定 y 軸座標

  const svg = d3
    .select(chartContainer.value)
    .append("svg")
    .attr("width", width)
    .attr("height", height);

  const path = svg.append("path").attr("class", "line");

  // 更新折線圖
  function updateLineChart() {
    xScale.domain([0, dataset.length - 1]);
    yScale.domain([0, d3.max(dataset, (d) => d.y)]);

    path
      .datum(dataset)
      .transition()
      .duration(500)
      .attr("d", line)
      .attr("fill", "none")
      .attr("stroke", "steelblue");
  }

  // 定期更新資料集並重繪折線圖
  intervalId = setInterval(() => {
    const newYValue = generateRandomValue();
    dataset.shift();
    dataset.push({ x: dataset.length, y: newYValue });
    updateLineChart();
  }, 300); // 每秒更新一次

  // 頁面卸載時清除定時器
  onUnmounted(() => {
    clearInterval(intervalId);
  });
});
</script>

<style scoped>
.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 2px;
}
</style>
