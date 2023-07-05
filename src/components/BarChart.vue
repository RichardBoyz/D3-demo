<template>
  <div ref="chartContainer"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

const chartContainer = ref(null);

onMounted(() => {
  const dataset = [
    { x: "A", y: 30 },
    { x: "B", y: 50 },
    { x: "C", y: 20 },
    { x: "D", y: 40 },
    { x: "E", y: 10 },
  ];

  const margin = { top: 20, right: 20, bottom: 30, left: 40 };
  const width = 400 - margin.left - margin.right;
  const height = 300 - margin.top - margin.bottom;

  const xScale = d3
    .scaleBand()
    .domain(dataset.map((d) => d.x))
    .range([0, width])
    .padding(0.1);

  const yScale = d3
    .scaleLinear()
    .domain([0, d3.max(dataset, (d) => d.y)])
    .range([height, 0]);

  const svg = d3
    .select(chartContainer.value)
    .append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", `translate(${margin.left}, ${margin.top})`);

  svg
    .selectAll(".bar")
    .data(dataset)
    .enter()
    .append("rect")
    .attr("class", "bar")
    .attr("x", (d) => xScale(d.x))
    .attr("y", (d) => yScale(d.y))
    .attr("width", xScale.bandwidth())
    .attr("height", (d) => height - yScale(d.y))
    .attr("fill", "steelblue");

  svg
    .append("g")
    .attr("class", "x-axis")
    .attr("transform", `translate(0, ${height})`)
    .call(d3.axisBottom(xScale));

  svg.append("g").attr("class", "y-axis").call(d3.axisLeft(yScale));

  svg
    .selectAll(".value")
    .data(dataset)
    .enter()
    .append("text")
    .attr("class", "value")
    .text((d) => d.y)
    .attr("x", (d) => xScale(d.x) + xScale.bandwidth() / 2)
    .attr("y", (d) => yScale(d.y) - 5)
    .attr("text-anchor", "middle");
});
</script>

<style scoped>
.bar {
  fill: steelblue;
}

.value {
  font-size: 12px;
  text-anchor: middle;
}
</style>
