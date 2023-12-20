<script setup lang="ts">
  const emit = defineEmits(['hideDropdown'])
defineProps({
    cursorPosition: Number
  })
</script>

<template>
  <div class="absolute w-[100px] bg-white flex flex-col p-3 border gap-2" :style="{'top': 10 + 'px', 'left': cursorPosition + 10 + 'px'}">
    <div @click="emit('hideDropdown')" class="cursor-pointer">Target</div>
    <div @click="emit('hideDropdown')" class="cursor-pointer">Deadline</div>
  </div>
</template>

<style scoped>

</style>