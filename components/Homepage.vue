<template>
  <main>
    <div
      class="flex justify-between items-center bg-yellow-200 w-full h-auto px-8"
    >
      <div class="flex items-center">
        <img src="~/assets/gitlab.svg" alt="logo" width="50" />
        <div class="font-bold text-lg">TeleAsk</div>
      </div>

      <p class="mx-2 font-bold">Room 201923</p>
    </div>

    <p class="font-bold text-lg bg-white text-center py-4 w-full h-auto">
      LifeHack 2021 Q&A Session
    </p>

    <md-tabs md-alignment="centered">
      <md-tab id="tab-unans" md-label="Unanswered">
        <NotAnswered :questions="unanswered" @refetch-text="fetchQuestions" />
      </md-tab>

      <md-tab id="tab-ans" md-label="Answered">
        <Answered :questions="answered" />
      </md-tab>
    </md-tabs>
  </main>
</template>

<script>
import { defineComponent, onMounted, ref } from '@nuxtjs/composition-api'
import Answered from '@/components/Answered.vue'
import NotAnswered from '@/components/NotAnswered.vue'

export default defineComponent({
  name: 'Homepage',
  components: {
    Answered,
    NotAnswered,
  },

  setup(props, { root }) {
    const answered = ref([])
    const unanswered = ref([])

    async function fetchQuestions() {
      try {
        answered.value = []
        unanswered.value = []
        const res = await root.$axios.get(
          'https://chong-testbot.herokuapp.com/retrieve'
        )
        const dataQueried = res.data

        for (const i of dataQueried) {
          // eslint-disable-next-line eqeqeq
          if (i.question_answer == undefined) {
            unanswered.value.push(i)
          } else {
            answered.value.push(i)
          }
        }
      } catch (error) {
        console.error(error)
      }
    }

    onMounted(() => {
      fetchQuestions()
    })

    return {
      fetchQuestions,
      answered,
      unanswered,
    }
  },
})
</script>
