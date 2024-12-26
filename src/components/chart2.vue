<template>
  <div ref="chart"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";
onMounted(() => {
  d3.select(chart.value).append("svg").attr("width", 4000).attr("height", 2000);
  getForce();
});

const chart = ref(null);

const getForce = () => {
  const nodes = [{ id: 1 }, { id: 2 }, { id: 3 }];

  const simulation = d3.forceSimulation(nodes);
  simulation.force("charge", d3.forceManyBody().strength(-30)); // 设置斥力强度为 -30
  simulation.force("center", d3.forceCenter(300, 300)); // 将节点吸引到画布中心 (300, 300)
  const links = [
    { source: 1, target: 2 },
    { source: 2, target: 3 },
  ];

  simulation.force("link", d3.forceLink(links).distance(100)); // 设置链接距离为 100
  simulation.force("collide", d3.forceCollide(50)); // 设置碰撞半径为 50
  simulation.alpha(0.5).alphaTarget(0); // 设置初始能量为 0.5，目标能量为 0

  simulation.on("tick", () => {
    d3.selectAll("circle")
      .attr("cx", (d) => d.x)
      .attr("cy", (d) => d.y);
  });
};
</script>

<style scoped></style>
