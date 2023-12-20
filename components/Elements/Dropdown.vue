<script setup lang="ts">
  const emit = defineEmits(['hideDropdown'])
  const props = defineProps({
    cursorPosition: Number
  })
  onMounted(()=>{
    console.log(props.cursorPosition)
  })
</script>

<template>
  <div class="absolute w-[100px]" :style="{'top': 10 + 'px'}">
    <div @click="emit('hideDropdown')">Target</div>
    <div @click="emit('hideDropdown')">Deadline</div>
  </div>
</template>

<style scoped>

</style>