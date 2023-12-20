<script setup lang="ts">
let value;
  const props = defineProps({
    selectedType: String,
  })
  const emit = defineEmits({
    'change': String
  })
</script>

<template>
  <div>
    <div>Target unit</div>
    <div class="gap-3 flex flex-row items-center justify-center">
      <input type="radio" id="percent" :checked="props.selectedType === 'default'" @change="emit('change', 'default')">
      <label for="percent">%</label>
      <input type="radio" id="custom" :checked="props.selectedType !== 'default'" @change="emit('change', 'MNOK')">
      <label for="custom">custom</label>
      <input v-if="props.selectedType !== 'default'" v-model="props.selectedType" class="input"/>
    </div>
  </div>
</template>

<style scoped>
 .input {
   padding: 2px;
   width: 80px;
 }
</style>