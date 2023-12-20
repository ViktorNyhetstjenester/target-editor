<script setup lang="ts">
  import type {ChangeEvent} from "rollup";

  const value = ref();
  const changeHandler = (event) => {
    console.log(event.target.value)
  }

</script>

<template>
  <input type="text" :value="value" @input="changeHandler"/>
</template>

<style scoped>

</style>