<script setup>
import { ref } from "vue";

const charts = ref({});
setTimeout(() => {
  const data = {
    Aug: 5,
    Jul: 0,
    Jun: 4,
    May: 3,
    Apr: 1,
    Mar: 12,
    Feb: 3,
    Jan: 4,
    Dec: 3,
    Nov: 5,
    Oct: 0,
    Sep: 2,
  };
  Object.assign(charts.value, data);
}, 3000);
import Chart from "./components/Chart.vue";
</script>

<template>
  <Chart :charts="charts" title="Chart Title" unit="Times" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
