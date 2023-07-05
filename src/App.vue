<script setup>
import { computed, ref } from "vue";
import BarChart from "./components/BarChart.vue";
import LineChart from "./components/LineChart.vue";
import PieChart from "./components/PieChart.vue";
import AnimateLine from "./components/AnimateLine.vue";
const currentChart = ref("bar");

const currentComponent = computed(() => {
  switch (currentChart.value) {
    case "bar":
      return BarChart;
    case "line":
      return LineChart;
    case "pie":
      return PieChart;
    case "ani":
      return AnimateLine;
    default:
      return BarChart;
  }
});
</script>

<template>
  <button @click="currentChart = 'bar'">顯示長條圖</button>
  <button @click="currentChart = 'line'">顯示折線圖</button>
  <button @click="currentChart = 'pie'">顯示圓餅圖</button>
  <button @click="currentChart = 'ani'">顯示動畫折線圖</button>

  <component :is="currentComponent" />
</template>

<style scoped></style>
