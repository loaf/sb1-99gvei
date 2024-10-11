<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  value: number;
  threshold: number;
}>();

const isAboveThreshold = computed(() => props.value > props.threshold);
</script>

<template>
  <div class="data-display">
    <h2>Current Value</h2>
    <p :class="{ 'above-threshold': isAboveThreshold }" :style="{ animation: isAboveThreshold ? 'blink 1s infinite' : 'none' }">
      {{ value.toFixed(2) }}
    </p>
  </div>
</template>

<style scoped>
.data-display {
  font-size: 24px;
  margin-bottom: 20px;
}

.above-threshold {
  color: red;
}

@keyframes blink {
  0% { opacity: 1; }
  50% { opacity: 0.5; }
  100% { opacity: 1; }
}
</style>