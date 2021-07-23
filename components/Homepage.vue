<template>
  <main>
    <div
      class="flex justify-between mb-4 items-center bg-yellow-200 w-full h-auto"
    >
      <div class="flex items-center">
        <img src="~/assets/gitlab.svg" alt="logo" width="50" />
        <div class="font-bold text-lg">Chongsters</div>
      </div>

      <p class="mx-2 font-bold">Room 201923</p>
    </div>
    <p class="font-bold mx-10 text-lg">LifeHack 2021 Question Session</p>

    <p class="font-semibold mx-10 bg-yellow-200 rounded-full w-max p-2">
      3 Questions
    </p>
    <div
      v-for="question in questions"
      :key="question.id"
      class="m-3 flex flex-wrap justify-center md:m-0 md:container"
    >
      <QuestionCard
        :id="question.question_id"
        :question="question.question_text"
        :answer="question.question_answer"
        :likes="question.subscription_count"
      />
    </div>
    <div class="flex justify-center">
      <p class="text-gray-500 text-xs absolute bottom-0 mb-4 mx-auto">
        &copy;2021 Chongsters. All rights reserved.
      </p>
    </div>
  </main>
</template>

<script>
import { defineComponent, onMounted, ref } from '@nuxtjs/composition-api'
import QuestionCard from '@/components/QuestionCard.vue'

export default defineComponent({
  name: 'Homepage',
  components: {
    QuestionCard,
  },
  setup(props, { root }) {
    const questions = ref()
    onMounted(() => {
      fetchQuestions()
    })
    async function fetchQuestions() {
      try {
        const res = await root.$axios.get(
          'https://chong-testbot.herokuapp.com/retrieve'
        )
        questions.value = res.data
        console.log(res.data)
      } catch (error) {
        console.error(error)
      }
    }

    return {
      fetchQuestions,
      questions,
    }
  },
})
</script>
