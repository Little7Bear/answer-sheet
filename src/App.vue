<template>
  <div class="wrapper">
    <h1 class="title">{{ title }}</h1>
    <template v-for="question in questionList" :key="question.name">
      <div>
        {{ question.name }}，正确：{{ question.count }}，错误{{ question.errorCount }}，{{ question.count }}/{{
          question.value[1] - question.value[0]
        }}
      </div>
      <ul class="list">
        <li v-for="(item, index) in list.slice(question.value[0], question.value[1])" :key="index" class="list-item">
          <div class="list-index">{{ question.value[0] + index + 1 }}.</div>
          <el-input v-model="item.value" class="list-input" @change="changeAnswer(item)" />
          <el-icon v-if="item.validity === 1" :size="20" color="#67C23A">
            <Check />
          </el-icon>
          <el-icon v-if="item.validity === 2" :size="20" color="#F56C6C">
            <Close />
          </el-icon>
        </li>
      </ul>
    </template>

    <div>
      <el-button class="btn" type="primary" @click="onCalc">计算答案</el-button>
      <div class="answer">正确：{{ count }}，错误{{ errorCount }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { Check, Close } from '@element-plus/icons-vue'

  type Item = {
    value: string
    validity: number
  }

  const list = ref<Item[]>([])
  for (let i = 0; i < 100; i++) {
    list.value.push({ value: '', validity: 0 })
  }

  fetchData()

  const answer = ref([
    'c',
    'c',
    'b',
    'a',
    'd',
    'd',
    'c',
    'a',
    'b',
    'd',
    'b',
    'd',
    'd',
    'b',
    'd',
    'c',
    'a',
    'a',
    'c',
    'd',
    'a',
    'b',
    'b',
    'c',
    'd',
    'b',
    'a',
    'b',
    'd',
    'c',
    'd',
    'b',
    'b',
    'c',
    'a',
    'a',
    'c',
    'b',
    'c',
    'a',
    'b',
    'd',
    'b',
    'a',
    'd',
    'c',
    'a',
    'b',
    'c',
    'd',
    'c',
    'd',
    'd',
    'c',
    'b',
    'a',
    'd',
    'a',
    'c',
    'c',
    'b',
    'b',
    'b',
    'c',
    'd',
    'b',
    'd',
    'b',
    'b',
    'd',
    'd',
    'a',
    'd',
    'd',
    'a',
    'c',
    'a',
    'a',
    'c',
    'c',
    'a',
    'b',
    'a',
    'c',
    'd',
    'c',
    'a',
    'b',
    'a',
    'd',
    'b',
    'b',
    'c',
    'a',
    'd',
    'b',
    'c',
    'b',
    'b',
    'a',
  ])
  let count = ref(0)
  let errorCount = ref(0)
  const onCalc = () => {
    let count1 = 0
    let count2 = 0
    list.value.forEach((item, i) => {
      if (answer.value[i] && item.value) {
        item.validity = answer.value[i] === item.value ? 1 : 2
        if (item.validity === 1) {
          count1++
        } else if (item.validity === 2) {
          count2++
        }
      }
    })
    count.value = count1
    errorCount.value = count2

    for (let i = 0; i < questionList.value.length; i++) {
      const item = questionList.value[i]
      item.count = 0
      for (let j = item.value[0]; j < item.value[1]; j++) {
        const jItem = list.value[j]
        if (jItem.validity === 1) {
          item.count++
        }
        if (jItem.validity === 2) {
          item.errorCount++
        }
      }
    }
  }

  const changeAnswer = (item: Item) => {
    if (item.value === '') {
      item.validity = 0
    }
    localStorage.setItem('answer', JSON.stringify(list.value))
  }

  const questionList = ref([
    { name: '常识判断', value: [0, 20], count: 0, errorCount: 0 },
    { name: '言语理解与表达', value: [20, 45], count: 0, errorCount: 0 },
    { name: '数量关系', value: [45, 50], count: 0, errorCount: 0 },
    { name: '判断推理', value: [50, 85], count: 0, errorCount: 0 },
    { name: '资料分析', value: [85, 100], count: 0, errorCount: 0 },
  ])

  const title = ref('2021年5月')

  function fetchData() {
    const data = localStorage.getItem('answer')
    if (data) {
      list.value = JSON.parse(data)
    }
  }
</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  ul {
    list-style: none;
  }

  .btn {
    margin-top: 16px;
  }

  .list {
    width: 1000px;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
    border: 1px solid #ccc;
    border-radius: 12px;
    padding: 16px;
  }

  .list-item {
    display: flex;
    align-items: end;
  }

  .list-index {
    width: 40px;
    margin-right: 10px;
    text-align: right;
  }

  .list-input {
    width: 50px;
  }

  .answer {
    margin-top: 10px;
    margin-left: 10px;
  }

  .title {
    font-size: 26px;
    line-height: 2;
  }
</style>
