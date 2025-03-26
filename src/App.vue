<template>
  <div class="w-full h-screen flex justify-center items-center bg-gray-200">
    <div class="bg-white shadow-xl rounded-lg p-8 text-center">
      <h1 class="text-3xl font-extrabold mb-6 text-gray-800">红绿灯模拟器</h1>
      <div
        class="text-5xl font-bold mb-6 w-32 h-32 flex items-center justify-center rounded-full m-auto"
        :class="{
          'bg-red-600 text-white': currentLight === '红灯',
          'bg-green-600 text-white': currentLight === '绿灯',
          'bg-yellow-600 text-white': currentLight === '黄灯',
        }"
      >
        {{ currentLight }}
      </div>
      <p class="text-gray-700 mb-2">当前灯：{{ currentLight }}</p>
      <p class="text-gray-500 mb-2">每秒打印当前灯的状态</p>
      <p class="text-gray-500 mb-2">红灯持续 10 秒，黄灯持续 3 秒，绿灯持续 8 秒</p>
      <p class="text-gray-500 mb-4">点击按钮重新开始</p>
      <button
        @click="restartTrafficLight"
        class="mt-4 bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 hover:scale-105 active:scale-95 transition-transform"
      >
        重新开始
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// 定义当前灯状态
const currentLight = ref('红灯')
let timer = null

// 红绿灯持续时间配置
const lightDurations = {
  红灯: 10000, // 红灯持续 10 秒
  黄灯: 3000,  // 黄灯持续 3 秒
  绿灯: 8000,  // 绿灯持续 8 秒
}

// 延迟函数
const sleep = (duration) => new Promise((resolve) => setTimeout(resolve, duration))

// 启动红绿灯逻辑
const performTrafficLight = async (startLight = '红灯') => {
  clearTimer() // 清理定时器
  currentLight.value = startLight
  logCurrentLight()

  try {
    while (true) {
      console.log(`当前灯：${currentLight.value}`)
      await sleep(lightDurations[currentLight.value]) // 等待当前灯的持续时间

      // 切换到下一个灯
      currentLight.value =
        currentLight.value === '红灯'
          ? '绿灯'
          : currentLight.value === '绿灯'
          ? '黄灯'
          : '红灯'
    }
  } finally {
    clearTimer()
  }
}

// 定时器：每秒打印当前灯的状态
const logCurrentLight = () => {
  console.log(`当前灯（等待中）：${currentLight.value}`)
  timer = setTimeout(logCurrentLight, 1000)
}

// 清理定时器
const clearTimer = () => {
  if (timer) {
    clearTimeout(timer)
    timer = null
  }
}

// 重新启动红绿灯
const restartTrafficLight = () => {
  performTrafficLight('红灯')
}

// 在组件挂载时启动红绿灯逻辑
onMounted(() => {
  performTrafficLight()
})

// 在组件卸载时清理定时器
onUnmounted(() => {
  clearTimer()
})
</script>