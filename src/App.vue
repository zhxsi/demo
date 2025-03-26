<template>
  <div>
    <p>当前灯：{{ currentLight }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// 定义当前灯状态
const currentLight = ref('红灯')
let timer = null

// 红绿灯逻辑
const performTrafficLight = async (startLight = '红灯') => {
  console.log('initialLight', startLight)
  currentLight.value = startLight // 初始为红灯

  const lightDurations = {
    '红灯': 10000, // 红灯持续 10 秒
    '黄灯': 3000,  // 黄灯持续 3 秒
    '绿灯': 8000   // 绿灯持续 8 秒
  }

  const sleep = (duration) => new Promise(resolve => setTimeout(resolve, duration))

  // 定时器：每秒打印当前灯的状态
  const logCurrentLight = () => {
    console.log(`当前灯（等待中）：${currentLight.value}`)
    timer = setTimeout(logCurrentLight, 1000)
  }
  logCurrentLight()

  try {
    while (true) {
      console.log(`当前灯：${currentLight.value}`)

      // 等待当前灯的持续时间
      await sleep(lightDurations[currentLight.value])

      // 根据当前灯切换到下一个灯
      switch (currentLight.value) {
        case '红灯':
          currentLight.value = '绿灯'
          break
        case '绿灯':
          currentLight.value = '黄灯'
          break
        case '黄灯':
          currentLight.value = '红灯'
          break
      }
    }
  } finally {
    // 清理定时器
    clearTimeout(timer)
  }
}

// 在组件挂载时启动红绿灯逻辑
onMounted(() => {
  performTrafficLight()
})

// 在组件卸载时清理定时器
onUnmounted(() => {
  if (timer) {
    clearTimeout(timer)
  }
})
</script>

<style scoped>
/* 添加样式（如果需要） */
</style>