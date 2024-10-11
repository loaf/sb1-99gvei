<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import io from 'socket.io-client';
import DataDisplay from './components/DataDisplay.vue';
import ThresholdSetter from './components/ThresholdSetter.vue';
import HistoricalChart from './components/HistoricalChart.vue';

const currentValue = ref(0);
const threshold = ref(50);
const historicalData = ref<{ timestamp: string; value: number }[]>([]);

const socket = io('http://localhost:3000'); // Adjust the URL to your server

onMounted(() => {
  socket.on('newData', (data: number) => {
    currentValue.value = data;
    historicalData.value.push({ timestamp: new Date().toISOString(), value: data });
    if (historicalData.value.length > 100) {
      historicalData.value.shift();
    }
  });

  fetchHistoricalData();
});

const fetchHistoricalData = async () => {
  try {
    const response = await axios.get('http://localhost:3000/historical-data');
    historicalData.value = response.data;
  } catch (error) {
    console.error('Error fetching historical data:', error);
  }
};

const updateThreshold = (newThreshold: number) => {
  threshold.value = newThreshold;
};
</script>

<template>
  <div class="dashboard">
    <h1>Real-time Data Dashboard</h1>
    <DataDisplay :value="currentValue" :threshold="threshold" />
    <ThresholdSetter :threshold="threshold" @update="updateThreshold" />
    <HistoricalChart :data="historicalData" />
  </div>
</template>

<style>
.dashboard {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}
</style>