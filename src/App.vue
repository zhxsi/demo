<template>
  <div>
    <button @click="count++">{{ count }}</button>
    <components1 :count="count" />1231
    <components2 :count="count" />
  </div>
</template>

<script setup>
import components1 from './components/components1/index.vue'
import components2 from './components/components2/index.vue'
import { ref } from 'vue'
const count = ref(0)
</script>

<style lang="scss" scoped></style>