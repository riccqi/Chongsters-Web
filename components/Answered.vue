<template>
  <main>
    <template v-if="questions.length !== 0">
      <div
        v-for="question in questions"
        :key="question.question_id"
        class="m-3 flex flex-wrap justify-center md:m-0"
      >
        <QuestionCard
          :id="question.question_id"
          :question="question.question_text"
          :answer="question.question_answer"
          :likes="question.subscription_count"
        />
      </div>
    </template>
    <div v-else-if="questions.length === 0">
      <p class="text-center font-semibold">No questions have been answered</p>
    </div>
    <div v-else>
      <spinner />
    </div>
    <div class="flex justify-center">
      <footer class="text-gray-500 text-xs mt-full mb-4">
        &copy;2021 Chongsters. All rights reserved.
      </footer>
    </div>
  </main>
</template>

<script>
import { defineComponent } from '@nuxtjs/composition-api'
import QuestionCard from '@/components/QuestionCard.vue'
import spinner from '~/assets/spinner.vue'

export default defineComponent({
  name: 'Answered',
  components: {
    QuestionCard,
    spinner,
  },
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  setup(props, { root }) {},
})
</script>
