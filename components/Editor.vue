<script setup lang="ts">

import {getCurrentInstance, onUnmounted, ref, defineProps, computed, watchEffect} from 'vue';
import renderComponent from "../helpers/RenderComponent";
import Dropdown from "./Elements/Dropdown.vue";
const props = defineProps({
  targetList: Array,
  currentEditable: Object,
});

const { appContext } = getCurrentInstance()
const container = ref()
const prevVal = ref();
const textAreaRef = ref();
let destroyComp: (() => void) | null = null
// let htmlToDom = ref();

onUnmounted(() => destroyComp?.())

const insert = async () => {
  destroyComp?.()
  destroyComp = renderComponent({
    el: container.value,
    component: (await import('~/components/Target.vue')).default,
    props: {
    },
    appContext,
  })
}
// onMounted(() => {
//   const parser = new DOMParser();
//   const htmlString = props.currentEditable?.value
//   htmlToDom.value = parser.parseFromString(htmlString, "text/html").body.children;
//   const elements = Array.from(htmlToDom.value)
//   const target: any = elements.find((item: any) => {
//     return item.attributes['data-type'].value
//   })
// })

const inputTextData = (event: any) => {
  event.target.innerHTML = event.target.innerHTML.replace('[target]', '<div>Hello</div>')
}

watchEffect(() => {
  if (prevVal.value !== props.currentEditable?.value) {
    prevVal.value = props.currentEditable?.value
    if(props.currentEditable?.value.includes('[target]') && props.currentEditable.value.includes('[deadline]')) {
      if(props.targetList?.length === 0) {
        props.targetList.push(
            {
              id: crypto.randomUUID(),
              value: props.currentEditable.value.split(' ')
            }
        );
      }
    }
  }
})

const cursor = computed(() => {
  getDropdownPosition()
})

const keyDown = (event: KeyboardEvent) => {
  switch (event.code) {
    case 'Slash': {
      showDropdown.value = true;
      getDropdownPosition();
      break;
    }
    case 'Enter': {
      event.preventDefault();
      break;
    }
    default: {
      break;
    }
  }
}

const showDropdown = ref(false);
const hideDropdown = () => {
  showDropdown.value = false
}

const getDropdownPosition = () => {
  const div = document.createElement('div');
  const styles = getComputedStyle(textAreaRef.value);
  for (const prop of styles) {
    div.style[prop] = styles[prop]
  }
  const textContent = textAreaRef.value.value.substring(0, textAreaRef.value.selectionStart)
  const span = document.createElement('span');
  span.textContent = textAreaRef.value.value.substr(textAreaRef.value.selectionStart) || '.';
  div.textContent = textContent;
  div.style.width = 'auto';
  div.appendChild(span);
  document.body.appendChild(div);
  const {offsetLeft} = span;
  document.body.removeChild(div);
  return offsetLeft
}

</script>

<template>
  <div @keydown="keyDown" class="flex flex-col w-[800px] min-h-[40px] rounded-sm  items-center relative">
    <ClientOnly>
      <textarea class="inline-block h-[40px] px-3 w-full bg-gray-100"
                rows="1"
                @input="inputTextData"
                placeholder="Start writing target description..."
                v-model="props.currentEditable.value"
                ref="textAreaRef"
      >
      </textarea>
      <Dropdown v-if="showDropdown" @hide-dropdown="hideDropdown" :cursorPosition="getDropdownPosition()"/>
    </ClientOnly>
    <component v-for="item in targetList"
                 :is="item.type"
                 :key="item.id"
                 v-bind="item"
                 :tabindex="item.id"
                 :id="item.id"
                 class="font-bold underline inline-block align-middle"
      >{{item}}</component>
    <div v-for="item in targetList">{{ item.value.join(' ') }}</div>
  </div>
</template>

<style scoped>
 textarea {
     border: none;
     overflow: hidden;
     outline: none;
     -webkit-box-shadow: none;
     -moz-box-shadow: none;
     box-shadow: none;
     resize: none;
 }
</style>