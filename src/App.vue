<script setup>
import chart1 from "./components/chart1.vue";
// import chart2 from "./components/chart2.vue";
import chart3 from "./components/chart3.vue";
import forceGraph from "./components/force-graph.vue";
import forceGraph2 from "./components/force-graph2.vue";
import scatterPlot from "./components/scatterplot-matrx/scatterplot.vue";
</script>

<template>
  <chart1 />
  <!-- <chart2 /> -->
  <force-graph />
  <forceGraph2 />
  <chart3 />
  <scatter-plot />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
