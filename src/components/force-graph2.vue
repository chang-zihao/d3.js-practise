<template>
  <div class="force-graph">
    <svg ref="svgRef2"></svg>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

// SVG 容器引用
const svgRef2 = ref(null);

//节点数据
const nodesList = [
  { id: "A", group: 1 },
  { id: "B", group: 1 },
  { id: "C", group: 1 },
  { id: "D", group: 1 },
  { id: "E", group: 2 },
  { id: "F", group: 2 },
  { id: "G", group: 2 },
  { id: "H", group: 2 },
  { id: "I", group: 2 },
  { id: "J", group: 3 },
  { id: "K", group: 4 },
  { id: "L", group: 5 },
];
const linksList = [
  { source: "A", target: "B" },
  { source: "A", target: "C" },
  { source: "A", target: "D" },
  { source: "B", target: "E" },
  { source: "B", target: "F" },
  { source: "C", target: "G" },
  { source: "C", target: "H" },
  { source: "D", target: "I" },
];
const width = 1000;
const height = 600;
onMounted(() => {
  const svg = d3
    .select(svgRef2.value)
    .attr("width", width)
    .attr("height", height);
  //力导图
  const simulation = d3
    .forceSimulation(nodesList)
    .force(
      "link",
      d3
        .forceLink(linksList)
        .id((d) => d.id)
        .distance(150)
    ) // 链接力
    .force("charge", d3.forceManyBody().strength(-300)) // 节点间的电荷力
    .force("center", d3.forceCenter(width / 2, height / 2)); // 中心力
  // 绘制链接线
  const link = svg
    .append("g")
    .attr("class", "links")
    .selectAll("line")
    .data(linksList)
    .enter()
    .append("line")
    .attr("stroke", "#999")
    .attr("stroke-width", 2);

  // 绘制节点
  const node = svg
    .append("g")
    .attr("class", "nodes")
    .selectAll("circle")
    .data(nodesList)
    .enter()
    .append("circle")
    .attr("r", 10)
    .attr("fill", (d) => d3.schemeCategory10[d.group])
    .call(
      d3
        .drag() // 拖拽交互
        .on("start", (event, d) => {
          if (!event.active) simulation.alphaTarget(0.3).restart();
          d.fx = d.x;
          d.fy = d.y;
        })
        .on("drag", (event, d) => {
          d.fx = event.x;
          d.fy = event.y;
        })
        .on("end", (event, d) => {
          if (!event.active) simulation.alphaTarget(0);
          d.fx = null;
          d.fy = null;
        })
    );
  // 节点标签
  const label = svg
    .append("g")
    .attr("class", "labels")
    .selectAll("text")
    .data(nodesList)
    .enter()
    .append("text")
    .attr("text-anchor", "middle")
    .attr("dy", -15)
    .text((d) => d.id)
    .attr("fill", "#333");
  // 每次 tick 更新位置
  simulation.on("tick", () => {
    link
      .attr("x1", (d) => d.source.x)
      .attr("y1", (d) => d.source.y)
      .attr("x2", (d) => d.target.x)
      .attr("y2", (d) => d.target.y);

    node.attr("cx", (d) => d.x).attr("cy", (d) => d.y);

    label.attr("x", (d) => d.x).attr("y", (d) => d.y);
  });
});
</script>

<style scoped>
.force-graph {
  display: flex;
  justify-content: center;
  align-items: center;
}
svg {
  border: 1px solid #ddd;
}
</style>
