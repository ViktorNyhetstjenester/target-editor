<script setup lang="ts">
import Target from './Target.vue';
import Deadline from './Deadline.vue'
const props = defineProps({
  input: Array
})
const filtered = (data: any) => {
  return data.join(' ').split(']').map(v => v.split('['))
}

</script>

<template>
  <div class="flex p-3 flex-col">
    <div class="flex">
      <img src="@/assets/target-29.svg" class="img">
      <div>Target</div>
    </div>
    <div v-for="item in props.input">
        <template v-for="[text, tag] in filtered(item.value)">
          {{text}}
          <template v-if="tag?.length && tag === 'target'">
            <Target>{{tag}}</Target>
          </template>
          <template v-if="tag?.length && tag === 'deadline'">
            <Deadline>{{tag}}</Deadline>
          </template>
        </template>
    </div>
  </div>


</template>

<style scoped>
.img {
  width: 24px;
}
</style>