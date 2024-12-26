<template>
  <div class="force-graph">
    <svg ref="svgRef"></svg>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

// 图数据
const nodes = [
  { id: "Node 1", group: 1 },
  { id: "Node 2", group: 1 },
  { id: "Node 3", group: 2 },
  { id: "Node 4", group: 2 },
  { id: "Node 5", group: 3 },
];

const links = [
  { source: "Node 1", target: "Node 2" },
  { source: "Node 1", target: "Node 3" },
  { source: "Node 3", target: "Node 4" },
  { source: "Node 4", target: "Node 5" },
];

// SVG 容器引用
const svgRef = ref(null);

onMounted(() => {
  const width = 800;
  const height = 600;

  // 创建 SVG 容器
  const svg = d3
    .select(svgRef.value)
    .attr("width", width)
    .attr("height", height);

  // 力导图布局
  const simulation = d3
    .forceSimulation(nodes)
    .force(
      "link",
      d3
        .forceLink(links)
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
    .data(links)
    .enter()
    .append("line")
    .attr("stroke", "#999")
    .attr("stroke-width", 2);

  // 绘制节点
  const node = svg
    .append("g")
    .attr("class", "nodes")
    .selectAll("circle")
    .data(nodes)
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
    .data(nodes)
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
