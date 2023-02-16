<script setup>
import {computed, ref, toRaw} from "vue";

const refLevelOne = ref([
  {
    value: 'zhejiang',
    label: 'Zhejiang',
    children: [
      {
        value: 'hangzhou',
        label: 'Hangzhou',
        children: [
          {
            value: 'xihu',
            label: 'West Lake',
          },
        ],
      },
      {
        value: 'jinhua',
        label: 'JinHua',
        children: [
          {
            value: 'yiwu',
            label: 'Xiao Shang Ping',
          },
          {
            value: 'dongyang',
            label: 'Heng Dian',
          }
        ],
      }
    ],
  },
  {
    value: 'jiangsu',
    label: 'Jiangsu',
    children: [
      {
        value: 'nanjing',
        label: 'Nanjing',
        children: [
          {
            value: 'zhonghuamen',
            label: 'Zhong Hua Men',
          },
        ],
      },
    ],
  }
])
let refSelectOne = ref(0)
let refSelectTwo = ref(0)
let refSelectThree = ref(0)

let refLevelTwo = computed(() => {
  return refLevelOne.value[refSelectOne.value].children || []
})
let refLevelThree = computed(() => {
  return refLevelTwo.value[refSelectTwo.value].children || []
})
const onSelectItemOne = (index) => {
  refSelectOne.value = index
  refSelectTwo.value = 0
  refSelectThree.value = 0
}
const onSelectItemTwo = (index) => {
  refSelectTwo.value = index
  refSelectThree.value = 0
}
const onSelectItemThree = (index) => {
  refSelectThree.value = index
}
</script>

<template>
  <div class="wrapper">
    <div class="item">
      <div v-for="(item, index) in refLevelOne" class="item-container" >
        <div class="checkbox">
          <input type="checkbox">
        </div>
        <div class="text" :class="[refSelectOne === index ? 'selected' : '']" @click="() => onSelectItemOne(index)">
          <span>{{ item.value }}</span>
        </div>
        <div class="arrow">></div>
      </div>
    </div>
    <div class="item">
      <div v-for="(item, index) in refLevelTwo" class="item-container">
        <div class="checkbox">
          <input type="checkbox">
        </div>
        <div class="text" :class="[refSelectTwo === index ? 'selected' : '']" @click="() => onSelectItemTwo(index)" >
          <span>{{ item.value }}</span>
        </div>
        <div class="arrow">></div>
      </div>
    </div>
    <div class="item">
      <div v-for="(item, index) in refLevelThree" class="item-container">
        <div class="checkbox">
          <input type="checkbox">
        </div>
        <div class="text" :class="[refSelectThree === index ? 'selected' : '']" @click="() => onSelectItemThree(index)">
          <span>{{ item.value }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.wrapper {
  padding: 8px;
  width: 60vw;
  height: 100vh;
  margin: 0 auto;
  border: 1px solid #c00;
  display: flex;
}

.wrapper .item {
  width: 30%;
  height: 100%;
  padding: 36px 30px;
  margin-right: 20px;
  border: 1px solid seagreen;
  flex: 1;
}

.wrapper .item:nth-child(3) {
  margin-right: 0;
}

.item-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.item-container .checkbox {
  width: 18px;
  height: 18px;
}

.item-container .checkbox input {
  width: 100%;
  height: 100%;
}

.item-container .text {
  width: 230px;
  height: 54px;
  border: 1px solid #4095e5;
  text-align: center;
  line-height: 54px;
}
.item-container .text.selected {
  border-color: #cc0000;
}

.item-container .arrow {
  font-size: 24px;
}
</style>