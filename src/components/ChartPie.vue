<template>
  <canvas ref="chartCanvas"></canvas>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { Chart, registerables } from "chart.js";

const chartCanvas = ref(null);
let chartInstance;

onMounted(() => {
  // 創建餅圖
  const ctx = chartCanvas.value.getContext("2d");
  chartInstance = new Chart(ctx, {
    type: "pie",
    data: {
      labels: ["Red", "Blue", "Yellow", "Green", "Purple"],
      datasets: [
        {
          label: "Data",
          data: [10, 20, 30, 40, 50],
          backgroundColor: ["red", "blue", "yellow", "green", "purple"],
          borderWidth: 1,
        },
      ],
    },
    options: {
      responsive: true,
    },
    plugins: [registerables],
  });
});
</script>
