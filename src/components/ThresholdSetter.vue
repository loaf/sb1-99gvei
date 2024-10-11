<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  threshold: number;
}>();

const emit = defineEmits<{
  (e: 'update', value: number): void;
}>();

const localThreshold = ref(props.threshold);

const updateThreshold = () => {
  emit('update', localThreshold.value);
};
</script>

<template>
  <div class="threshold-setter">
    <h2>Set Threshold</h2>
    <input type="number" v-model="localThreshold" @change="updateThreshold" />
  </div>
</template>

<style scoped>
.threshold-setter {
  margin-bottom: 20px;
}

input {
  font-size: 18px;
  padding: 5px;
}
</style>