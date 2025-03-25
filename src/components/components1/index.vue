<template>
  <div>
    {{ count }}
  </div>
</template>

<script setup>
const { count } = defineProps({
  count: Number
})

</script>

<style lang="scss" scoped></style>