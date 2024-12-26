<template>
  <div ref="chart"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

defineProps({
  msg: String,
});
onMounted(() => {
  d3.select(chart.value).append("svg").attr("width", 4000).attr("height", 600);
  getCircle();
  getLine();
  getArc();
  getArea();
  getPie();
  getSymbol();
  getLineRadial();
  getLinkHorizontal();
});
const chart = ref(null);
/* 圆形 */
const getCircle = () => {
  d3.select("svg")
    .append("circle")
    .attr("cx", 200)
    .attr("cy", 100)
    .attr("r", 50)
    .attr("fill", "blue");
};
/* 线 */
const getLine = () => {
  const data = [
    { x: 0, y: 50 },
    { x: 50, y: 100 },
    { x: 100, y: 150 },
  ];

  const lineGenerator = d3
    .line()
    .x((d) => d.x)
    .y((d) => d.y);

  const pathData = lineGenerator(data);

  d3.select("svg")
    .append("path")
    .attr("d", pathData)
    .attr("stroke", "black")
    .attr("fill", "none")
    .attr("transform", "translate(400, 10)");
};
/* 弧形 */
const getArc = () => {
  const arcGenerator = d3
    .arc()
    .innerRadius(50)
    .outerRadius(90)
    .startAngle(0)
    .endAngle(Math.PI / 2);

  const pathData = arcGenerator();

  d3.select("svg").append("path").attr("d", pathData).attr("fill", "blue");
};
/* 面积 */
const getArea = () => {
  const data = [
    { x: 0, y0: 0, y1: 100 },
    { x: 50, y0: 70, y1: 120 },
    { x: 100, y0: 30, y1: 190 },
  ];

  const areaGenerator = d3
    .area()
    .x((d) => d.x)
    .y0((d) => d.y0)
    .y1((d) => d.y1);

  const pathData = areaGenerator(data);

  d3.select("svg").append("path").attr("d", pathData).attr("fill", "lightblue");
};
const getPie = () => {
  const data = [10, 20, 30, 40];

  const pieGenerator = d3.pie();
  const pieData = pieGenerator(data);

  const arcGenerator = d3.arc().innerRadius(0).outerRadius(100);

  d3.select("svg")
    .selectAll("path")
    .data(pieData)
    .enter()
    .append("path")
    .attr("d", arcGenerator)
    .attr("fill", (d, i) => d3.schemeCategory10[i])
    .attr("transform", "translate(300, 100)");
};

const getSymbol = () => {
  const symbolGenerator = d3.symbol().type(d3.symbolStar).size(200);

  const pathData = symbolGenerator();

  d3.select("svg")
    .append("path")
    .attr("d", pathData)
    .attr("transform", "translate(100, 100)")
    .attr("fill", "orange")
    .attr("transform", "translate(600, 100)");
};

const getLineRadial = () => {
  const data = [
    { angle: 0, radius: 50 },
    { angle: Math.PI / 4, radius: 70 },
    { angle: Math.PI / 2, radius: 100 },
  ];

  const radialLineGenerator = d3
    .lineRadial()
    .angle((d) => d.angle)
    .radius((d) => d.radius);

  const pathData = radialLineGenerator(data);

  d3.select("svg")
    .append("path")
    .attr("d", pathData)
    .attr("stroke", "green")
    .attr("fill", "none")
    .attr("transform", "translate(700, 100)");
};

const getLinkHorizontal = () => {
  const linkGenerator = d3
    .linkHorizontal()
    .source((d) => d.source)
    .target((d) => d.target);

  const linkData = {
    source: [0, 0],
    target: [100, 100],
  };

  const pathData = linkGenerator(linkData);

  d3.select("svg")
    .append("path")
    .attr("d", pathData)
    .attr("stroke", "black")
    .attr("fill", "none")
    .attr("transform", "translate(900, 10)");
};
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
