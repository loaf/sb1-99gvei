<script setup lang="ts">
import { computed } from 'vue';
import { Line } from 'vue-chartjs';
import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend } from 'chart.js';

ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend);

const props = defineProps<{
  data: { timestamp: string; value: number }[];
}>();

const chartData = computed(() => ({
  labels: props.data.map(item => new Date(item.timestamp).toLocaleTimeString()),
  datasets: [
    {
      label: 'Historical Data',
      data: props.data.map(item => item.value),
      borderColor: 'rgb(75, 192, 192)',
      tension: 0.1
    }
  ]
}));

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false
};
</script>

<template>
  <div class="historical-chart">
    <h2>Historical Data</h2>
    <Line :data="chartData" :options="chartOptions" />
  </div>
</template>

<style scoped>
.historical-chart {
  height: 400px;
}
</style>