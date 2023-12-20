<script setup lang="ts">
import Editor from "~/components/Editor.vue";
import Preview from "~/components/Preview.vue";
import DeadlinePropertiesElement from "~/components/Elements/DeadlinePropertiesElement.vue";
import TargetPropertiesElement from "~/components/Elements/TargetPropertiesElement.vue";
import {ref, reactive} from "vue";
import Button from "~/components/shared/Button.vue";
const TargetDef = reactive({
  baseLine: {},
  targets: []
});

const targetPlaced = ref(false);
const deadLinePlaced = ref(false);
const targetTypeSelected = ref('default');
const deadLineFormatSelected = ref('Year');

const setTargetPlaced = () => {
  targetPlaced.value = true;
}

const setDeadLinePlaced = () => {
  deadLinePlaced.value = true;
}
const CurrentEditable = reactive({
  value: ''
})
const addNewTarget = (target: any) => {
  CurrentEditable.value += ' [target] ';
  setTargetPlaced();
}

const addNewDeadline = () => {
  CurrentEditable.value += ' [deadline] ';
  setDeadLinePlaced();
}

const addTarget = () => {
  TargetDef.targets.push(CurrentEditable)
}

</script>

<template>
  <div class="gap-8 flex flex-row">
    <div>
      <div class="flex flex-row justify-between my-1">
        <div>Target definition</div>
        <div class="gap-2 flex">
          <Button @click="addNewTarget" v-if="!targetPlaced">Add target</Button>
          <Button @click="addNewDeadline" v-if="!deadLinePlaced">Add deadline</Button>
        </div>
      </div>
      <Editor :targetList="TargetDef.targets" :currentEditable="CurrentEditable"></Editor>
      <div class="flex flex-row gap-6">
        <div v-if="targetPlaced"><TargetPropertiesElement :selectedType="targetTypeSelected" @change="(val) => {
          targetTypeSelected = val;
        }"/></div>
        <div v-if="deadLinePlaced"><DeadlinePropertiesElement :selectedFormat="deadLineFormatSelected" @change="(val) => {
          deadLineFormatSelected = val;
        }"/></div>
      </div>
    </div>
    <div class="bg-gray-100 w-[350px] min-w-[350px] rounded-lg border-gray-300 border mt-1">
      <Preview :input="TargetDef.targets"/>
    </div>
  </div>

</template>

<style scoped>

</style>