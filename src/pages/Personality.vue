<template>
  <div class="px-[40px] w-full">
    <div class="pt-[120px] flex gap-1">
      <div v-for="(items, index) in questions" :key="index" class="w-full h-[8px] border border-radius rounded-[4px]" :style="{ 'background-color': currentIndex >= index ? '#333333' : 'white' }"></div>
    </div>
    <div class="question-container font-bold mt-[45px]">
      <p class="font-[HandWrite] text-[#C8C8C8] text-[32px]">Q{{ questions[currentIndex].id }}</p>
      <p>{{ questions[currentIndex].questionName }}</p>
      <div class="mt-[45px]">
        <button v-for="(option, index) in questions[currentIndex].option" :key="index" @click="handleSelectAnswer(option)" class="w-full py-[20px] border border-[#D9D9D9] rounded-[20px] mt-[14px] align-left" :style="{ 'background-color': selectAnswer.id === option.id ? '#F57C4A' : 'white' }">{{ option.questionText }}</button>
      </div>
    </div>
    <div class="flex items-center gap-[20px]">
      <button @click="goBack()" class="flex w-[40%] justify-center items-center border border-[#000] rounded-[20px] bg-white h-[40px] mt-[80px]">กลับ</button>
      <button @click="goNext()" class="flex w-[70%] justify-center items-center border border-[#000] rounded-[20px] bg-[#F7C116] h-[40px] mt-[80px]">ต่อไป</button>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { reactive, ref, watch } from 'vue'
  import router from '../router/index'

  interface QuestionOption {
    id: number
    type: string
    questionName: string
    option: Array<AnswersDetail>
  }
  interface AnswersDetail {
    questionText: string
    score: number
    id: number
  }
  interface Answers {
    id: number
    answer: Array<AnswersDetail>
  }

  const questions = reactive<QuestionOption[]>([
    {
      id: 1,
      type: 'single',
      questionName: 'ถ้าวันนี้เป็นวันหยุด คุณจะ...',
      option: [
        { questionText: 'นอนเล่น พักผ่อน', score: 10, id: 1 },
        { questionText: 'ดูหนัง ดูซีรี่ส์ เล่นเกม', score: 20, id: 2 },
        { questionText: 'เรียนรู้อะไรใหม่ ๆ', score: 30, id: 3 },
      ],
    },
    {
      id: 2,
      type: 'single',
      questionName: 'คุณต้องการพัฒนาภาษาอังกฤษเพื่ออะไร?',
      option: [
        { questionText: 'การทำงาน', score: 30, id: 1 },
        { questionText: 'เที่ยวต่างประเทศ', score: 10, id: 2 },
        { questionText: 'ใช้ในชีวิตประจำวัน', score: 20, id: 3 },
      ],
    },
    {
      id: 3,
      type: 'single',
      questionName: 'สไตล์การเรียนภาษาอังกฤษของคุณเป็นแบบไหน?',
      option: [
        { questionText: 'เรียนคอร์สออนไลน์ / โรงเรียนสอนภาษา', score: 30, id: 1 },
        { questionText: 'เรียนด้วยตนเอง เช่น อ่านหนังสือ ดูหนัง ฟังเพลง', score: 20, id: 2 },
        { questionText: 'คุยกับเพื่อนต่างชาติ / ไปเที่ยวต่างประเทศ', score: 10, id: 3 },
      ],
    },
    {
      id: 4,
      type: 'single',
      questionName: 'คุณใช้ภาษาอังกฤษบ่อยแค่ไหน?',
      option: [
        { questionText: 'เป็นประจำทุกวัน', score: 30, id: 1 },
        { questionText: 'เป็นบางครั้ง', score: 10, id: 2 },
        { questionText: 'นาน ๆ ที / ไม่ได้ใช้เลย', score: 20, id: 3 },
      ],
    },
    {
      id: 5,
      type: 'single',
      questionName: 'คุณอยากพัฒนาสกิลภาษาอังกฤษสกิลไหนมากที่สุด?',
      option: [
        { questionText: 'การฟัง', score: 20, id: 1 },
        { questionText: 'การพูด', score: 10, id: 2 },
        { questionText: 'การเขียน', score: 30, id: 3 },
      ],
    },
    {
      id: 6,
      type: 'single',
      questionName: 'อยากเรียนภาษาอังกฤษ แต่...',
      option: [
        { questionText: 'ไม่มีเวลา', score: 20, id: 1 },
        { questionText: 'เนื้อหาไม่น่าสนใจ / สอนไม่สนุก', score: 10, id: 2 },
        { questionText: 'ไม่มั่นใจ', score: 30, id: 3 },
      ],
    },
    {
      id: 7,
      type: 'multiple',
      questionName: 'คุณรู้จัก Globish ผ่านช่องทางใด(เลือกได้มากกว่า 1 ข้อ)',
      option: [
        { questionText: 'Instagram', score: 0, id: 1 },
        { questionText: 'Facebook', score: 0, id: 2 },
        { questionText: 'อื่นๆ (โปรดระบุ)', score: 0, id: 3 },
      ],
    },
  ])

  const currentIndex = ref<number>(0)
  const selectAnswer = reactive<AnswersDetail>({
    questionText: '',
    score: 0,
    id: 0,
  })
  const totalScore = ref<number>(0)
  const getAnswer = reactive<Answers[]>([])

  const goBack = () => {
    if (currentIndex.value > 0) {
      ;(selectAnswer.id = 0), (selectAnswer.score = 0), (selectAnswer.questionText = ''), (currentIndex.value -= 1)
    } else {
      router.push('/')
    }
  }
  const goNext = () => {
    if (currentIndex.value < questions.length - 1) {
      getAnswer.push({ id: 1, answer: [{ ...selectAnswer }] })
      ;(selectAnswer.id = 0), (selectAnswer.score = 0), (selectAnswer.questionText = ''), (currentIndex.value += 1)
      console.log(getAnswer)
    }
  }

  const handleSelectAnswer = (option: any) => {
    const tempSelectAnswer = option
    Object.assign(selectAnswer, tempSelectAnswer)
    console.log(selectAnswer)
  }

  watch(currentIndex, () => {
    const tempSelectAnswer = getAnswer[currentIndex.value]
    Object.assign(selectAnswer, tempSelectAnswer)
  })
</script>

<style scoped>
  .question-container {
    background: white;
    padding: 25px;
    border: 1px solid;
    border-radius: 20px;
    width: 100%;
    height: 500px;
  }
</style>
