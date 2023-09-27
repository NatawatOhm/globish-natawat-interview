<template>
  <div class="px-[9px] pt-[60px]">
    <div class="flex flex-col items-center w-full bg-white border rounded-[20px] py-[40px]">
      <p class="text-[36px] font-bold">ผลลัพธ์ของคุณคือ</p>
      <div v-if="totalScore != null && totalScore > 130">
        <img :src="gradualImage" alt="" />
      </div>
      <div v-if="totalScore != null && totalScore > 90 && totalScore < 129">
        <img :src="enthusiasticImage" alt="" />
      </div>
      <div v-if="totalScore != null && totalScore > 59 && totalScore < 89">
        <img :src="hobbyistImage" alt="" />
      </div>
      <div class="text-[20px] font-bold flex flex-col items-center pt-[50px]">
        <p>คุณรู้จัก Globish ผ่าน</p>
        <p>{{ platform }}, {{ otherOption }}</p>
      </div>
    </div>
    <div class="flex justify-center">
      <button @click="goMain()" class="flex w-[50%] justify-center items-center border rounded-[20px] bg-[#F7C116] h-[40px] mt-[80px]">กลับสู่หน้าแรก</button>
    </div>
  </div>
</template>

<script setup lang="ts">
  import gradualImage from '../assets/image/gradual-learner.png'
  import enthusiasticImage from '../assets/image/enthusiastic.png'
  import hobbyistImage from '../assets/image/hobbyist.png'
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'

  const router = useRouter()
  const platform = ref<string>('Instagram')
  const otherOption = ref<string>('เพื่อนแนะนำมา')

  const totalScore = parseInt(router.currentRoute.value.query.totalScore as string)

  const goMain = () => {
    router.push('/')
  }
</script>

<style scoped></style>
