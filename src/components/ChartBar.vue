<template>
  <canvas ref="chartCanvas"></canvas>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { Chart, registerables } from "chart.js";

const chartCanvas = ref(null);
let chartInstance;
Chart.register(...registerables);

onMounted(() => {
  // 創建長條圖
  const ctx = chartCanvas.value.getContext("2d");
  chartInstance = new Chart(ctx, {
    type: "bar",
    data: {
      labels: ["A", "B", "C", "D", "E"],
      datasets: [
        {
          label: "Data",
          data: [10, 20, 30, 40, 50],
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
