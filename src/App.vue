<template>
  <div class="wrapper">
    <ul class="list">
      <li v-for="(item, index) in list" :key="index" class="list-item">
        <div class="list-index">{{ index + 1 }}.</div>
        <el-input v-model="item.value" class="list-input" />
        <el-icon v-if="item.validity === 1" :size="20" color="#67C23A">
          <Check />
        </el-icon>
        <el-icon v-if="item.validity === 2" :size="20" color="#F56C6C">
          <Close />
        </el-icon>
      </li>
    </ul>

    <div>
      <el-button class="btn" type="primary" @click="onCalc">计算答案</el-button>
      <div class="answer">正确：{{ count }}，错误{{ errorCount }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue'
  import { Check, Close } from '@element-plus/icons-vue'

  const list = ref<{ value: string; validity: number }[]>([])
  for (let i = 0; i < 100; i++) {
    list.value.push({ value: '', validity: 0 })
  }

  const answer = ref(['a'])
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

  .wrapper {
    display: flex;
  }

  .btn {
    margin-left: 16px;
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
</style>
