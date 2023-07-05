<template>
  <canvas ref="chartCanvas"></canvas>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { Chart, registerables } from "chart.js";

const chartCanvas = ref(null);
let chartInstance;

onMounted(() => {
  // 創建折線圖
  const ctx = chartCanvas.value.getContext("2d");
  chartInstance = new Chart(ctx, {
    type: "line",
    data: {
      labels: ["Jan", "Feb", "Mar", "Apr", "May"],
      datasets: [
        {
          label: "Data",
          data: [50, 30, 40, 20, 10],
          backgroundColor: "rgba(75, 192, 192, 0.5)",
          borderColor: "rgba(75, 192, 192, 1)",
          borderWidth: 1,
        },
      ],
    },
    options: {
      responsive: true,
      scales: {
        y: {
          beginAtZero: true,
        },
      },
    },
    plugins: [registerables],
  });
});
</script>
