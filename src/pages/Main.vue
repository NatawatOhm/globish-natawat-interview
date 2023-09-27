<template>
  <div class="bg-container flex justify-center">
    <button @click="goNext()" class="flex w-[80%] justify-center items-center border border-[#000] rounded-[20px] bg-[#F7C116] h-[40px] mb-[85px] absolute bottom-0">เริ่มเลย</button>
  </div>
</template>

<script setup lang="ts">
  import route from '../router/index'

  const goNext = () => {
    route.push('/personality')
  }
</script>

<style scoped>
  .bg-container {
    height: 100vh;
    width: 100%;
    background-image: url('@/assets/image/get-start.png');
    background-repeat: no-repeat;
    background-position: center;
  }
</style>
