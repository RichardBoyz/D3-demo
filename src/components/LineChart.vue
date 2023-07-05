<template>
  <div ref="chartContainer"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

const chartContainer = ref(null);

onMounted(() => {
  const dataset = [
    { x: 0, y: 5 },
    { x: 1, y: 9 },
    { x: 2, y: 7 },
    { x: 3, y: 3 },
    { x: 4, y: 8 },
  ];

  const margin = { top: 20, right: 20, bottom: 30, left: 40 };
  const width = 400 - margin.left - margin.right;
  const height = 300 - margin.top - margin.bottom;

  const xScale = d3
    .scaleLinear()
    .domain([0, d3.max(dataset, (d) => d.x)])
    .range([0, width]);

  const yScale = d3
    .scaleLinear()
    .domain([0, d3.max(dataset, (d) => d.y)])
    .range([height, 0]);

  const line = d3
    .line()
    .x((d) => xScale(d.x))
    .y((d) => yScale(d.y));

  const svg = d3
    .select(chartContainer.value)
    .append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", `translate(${margin.left}, ${margin.top})`);

  svg
    .append("path")
    .datum(dataset)
    .attr("fill", "none")
    .attr("stroke", "blue")
    .attr("d", line);

  svg
    .selectAll(".dot")
    .data(dataset)
    .enter()
    .append("circle")
    .attr("class", "dot")
    .attr("cx", (d) => xScale(d.x))
    .attr("cy", (d) => yScale(d.y))
    .attr("r", 10)
    .attr("fill", "red");

  svg
    .append("g")
    .attr("class", "x-axis")
    .attr("transform", `translate(0, ${height})`)
    //.call(d3.axisBottom(xScale).ticks(5));
    .call(d3.axisBottom(xScale).tickFormat(d3.format(".1f")));

  svg.append("g").attr("class", "y-axis").call(d3.axisLeft(yScale));

  svg
    .selectAll(".value")
    .data(dataset)
    .enter()
    .append("text")
    .attr("class", "value")
    .text((d) => `(${d.x}, ${d.y})`)
    .attr("x", (d) => xScale(d.x) + 10)
    .attr("y", (d) => yScale(d.y) - 5)
    .attr("text-anchor", "start");
});
</script>

<style scoped>
.line {
  fill: none;
  stroke: steelblue;
  stroke-width: 2px;
}

.dot {
  fill: steelblue;
}

.value {
  font-size: 12px;
}
</style>
