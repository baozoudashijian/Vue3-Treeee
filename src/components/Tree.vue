<script setup>
import {computed, ref, toRaw, onMounted} from "vue";

const refLevelOne = ref([
  {
    value: 'zhejiang',
    label: 'Zhejiang',
    children: [
      {
        checked: true,
        value: 'hangzhou',
        label: 'Hangzhou',
        children: [
          {
            checked: true,
            value: 'xihu',
            label: 'West Lake',
          },
        ],
      },
      {
        checked: false,
        value: 'jinhua',
        label: 'JinHua',
        children: [
          {
            checked: true,
            value: 'yiwu',
            label: 'Xiao Shang Ping',
          },
          {
            checked: false,
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
            checked: true,
            value: 'zhonghuamen',
            label: 'Zhong Hua Men',
          },
          {
            checked: false,
            value: 'suzhou',
            label: 'Su Zhou Yuan Lin',
          }
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
const initalChecked = (data) => {
  return toRaw(data).map(item => {
    if(item.children) {
      initalChecked(item.children)
    } else {
      return
    }
    item.checked = item.children.every(item => item.checked)
    return item
  })
}
onMounted(() => {
  refLevelOne.value = initalChecked(refLevelOne.value)
})
const onSelectAllOne = (index, e) => {
  const checked = e.target.checked
  const arr = toRaw(refLevelOne.value)
  arr[index].checked = checked
  arr[index].children = selectAll(arr[index].children, checked)
  refLevelOne.value =  JSON.parse(JSON.stringify(arr))
  onSelectItemOne(index)
}
const onSelectAllTwo = (index, e) => {
  const checked = e.target.checked
  const arr = toRaw(refLevelOne.value)
  const target = arr[refSelectOne.value].children
  target[index].checked = checked
  target[index].children = selectAll(target[index].children, checked)
  refLevelOne.value =  JSON.parse(JSON.stringify(arr))
  onSelectItemTwo(index)
  refLevelOne.value = initalChecked(refLevelOne.value)
  
}
const onSelectAllTree = (index, e) => {
  const checked = e.target.checked
  const arr = toRaw(refLevelOne.value)
  const target = arr[refSelectOne.value].children[refSelectTwo.value].children
  target[index].checked = checked
  refLevelOne.value =  JSON.parse(JSON.stringify(arr))
  onSelectItemThree(index)
  refLevelOne.value = initalChecked(refLevelOne.value)
  
}
const selectAll = (data, checked) => {
  return data.map(item => {
    if(item.children) {
      selectAll(item.children, checked)
    }
    item.checked = checked
    return item
  })
}
</script>

<template>
  <div class="wrapper">
    <div class="item">
      <div v-for="(item, index) in refLevelOne" class="item-container" >
        <div class="checkbox" @click="(e) => onSelectAllOne(index, e)">
          <input type="checkbox" :checked="item.checked">
        </div>
        <div class="text" :class="[refSelectOne === index ? 'selected' : '']" @click="() => onSelectItemOne(index)">
          <span>{{ item.value }}</span>
        </div>
        <div class="arrow">></div>
      </div>
    </div>
    <div class="item">
      <div v-for="(item, index) in refLevelTwo" class="item-container">
        <div class="checkbox" @click="(e) => onSelectAllTwo(index, e)">
          <input type="checkbox" :checked="item.checked">
        </div>
        <div class="text" :class="[refSelectTwo === index ? 'selected' : '']" @click="() => onSelectItemTwo(index)" >
          <span>{{ item.value }}</span>
        </div>
        <div class="arrow">></div>
      </div>
    </div>
    <div class="item">
      <div v-for="(item, index) in refLevelThree" class="item-container">
        <div class="checkbox" @click="(e) => onSelectAllTree(index, e)">
          <input type="checkbox" :checked="item.checked">
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
  flex-grow: 1;
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