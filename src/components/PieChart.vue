<template>
  <div ref="chartContainer"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

const chartContainer = ref(null);

onMounted(() => {
  const dataset = [30, 50, 20];

  const width = 400;
  const height = 400;
  const radius = Math.min(width, height) / 2;

  const color = d3
    .scaleOrdinal()
    .domain(dataset)
    .range(["#ff7f0e", "#2ca02c", "#1f77b4"]);

  const arc = d3.arc().innerRadius(0).outerRadius(radius);

  // 圓形範圍生成
  const pie = d3
    .pie() // 計算數值在圓餅中所占用的角度，並生成圓餅圖的相關數據
    .value((d) => d) // 讀取資料中每個數值並直接回傳
    .sort(null); // 可以使用 sort((a,b)=>a-b) 的方式排序資料

  const svg = d3
    .select(chartContainer.value) // 選擇圖表容器元素
    .append("svg") // 在容器內創建 SVG 元素
    .attr("width", width)
    .attr("height", height)
    .append("g") // 在 SVG 元素內創建一個群組（group）元素
    .attr("transform", `translate(${width / 2}, ${height / 2})`); // 將群組元素移動到 SVG 的中心位置

  const arcs = svg
    .selectAll(".arc") // 選擇所有具有 "arc" 類別的元素（如果不存在，則選擇空集）
    .data(pie(dataset)) // 將數據集應用於圓餅圖生成器，並生成用於繪製圓餅圖的數據結構
    .enter() // 開始綁定數據到選擇集，並返回與數據結構中未使用的元素相對應的占位符節點
    .append("g") // 為每個占位符節點創建一個 <g> 元素，用於包含圓餅圖的每個部分
    .attr("class", "arc") // 將 "arc" 類別添加到每個 <g> 元素，用於樣式化和選擇
    .on("click", handleArcClick); // 設置點擊事件處理，當點擊圓餅圖的一部分時觸發

  arcs
    .append("path") // 在每個 <g> 加上 <path> 元素
    .attr("d", arc) // 設定 <path> 元素的 d 屬性，定義圓弧形狀
    .attr("fill", (d) => color(d.data)); // 設定圓弧形狀面積顏色

  arcs
    .append("text") // 在每個 <g> 加上一個 <text> 元素
    .attr("transform", (d) => `translate(${arc.centroid(d)})`) // 設定 text 出現的位置，範例是在各個面積中間
    .attr("dy", "0.35em") // 調整 text 的垂直位置來對其畫面
    .text((d) => `${d.data}%`) // 設定 text 的顯示文字
    .style("text-anchor", "middle"); // 設定 css 中 text 的水平對其方式

  function handleArcClick(event, d) {
    console.log(this);
    const clickedArc = d3.select(this);

    // 生成隨機顏色
    const randomColor = getRandomColor();

    // 將被點擊的區塊顏色變換為隨機顏色
    clickedArc
      .select("path")
      .transition()
      .duration(500)
      .attr("fill", randomColor);
  }

  function getRandomColor() {
    const letters = "0123456789ABCDEF";
    let color = "#";
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }
});
</script>

<style scoped>
.arc text {
  font-size: 12px;
}
</style>
