<template>
  <main>
    <div class="flex justify-between items-center bg-yellow-200 w-full h-auto">
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
        <NotAnswered />
      </md-tab>

      <md-tab id="tab-ans" md-label="Answered">
        <Answered />
      </md-tab>
    </md-tabs>
  </main>
</template>

<script>
import { defineComponent } from '@nuxtjs/composition-api'
import Answered from '@/components/Answered.vue'
import NotAnswered from '@/components/NotAnswered.vue'

export default defineComponent({
  name: 'Homepage',
  components: {
    Answered,
    NotAnswered,
  },

  setup(props, { root }) {},
})
</script>
