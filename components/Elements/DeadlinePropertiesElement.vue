<script setup lang="ts">
const props = defineProps({
  selectedFormat: String,
});
const emit = defineEmits({
  'change': String
})
const values = ['Year', 'Month', 'Day'];
</script>

<template>
<div>Deadline</div>
  <div class="gap-2 flex flex-row items-center justify-center">
  <template v-for="value in values">
    <input type="radio" :name="value" :id="value" :value="value" :checked="selectedFormat === value" @click="emit('change', value)"/>
    <label :for="value">{{value}}</label>
  </template>
  </div>
</template>

<style scoped>

</style>