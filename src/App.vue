<script setup>
import { computed, ref } from "vue";
import BarChart from "./components/BarChart.vue";
import LineChart from "./components/LineChart.vue";
import PieChart from "./components/PieChart.vue";
import AnimateLine from "./components/AnimateLine.vue";
import ChartBar from "./components/ChartBar.vue";
import ChartLine from "./components/ChartLine.vue";
import ChartPie from "./components/ChartPie.vue";
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
    case "charBar":
      return ChartBar;
    case "charLine":
      return ChartLine;
    case "charPie":
      return ChartPie;
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
  <button @click="currentChart = 'charBar'">Chartjs 長條圖</button>
  <button @click="currentChart = 'charLine'">Chartjs 折線圖</button>
  <button @click="currentChart = 'charPie'">Chartjs 圓餅圖</button>

  <component :is="currentComponent" />
</template>

<style scoped></style>
