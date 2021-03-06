<template>
  <main class="flex items-center shadow-md mb-6 rounded-xl overflow-hidden">
    <div
      class="
        font-bold
        px-2.5
        bg-blue-500
        h-full
        text-white
        flex
        justify-center
        flex-col
        w-8
      "
    >
      {{ id }}
    </div>
    <div class="p-3 flex-grow w-80 md:w-96">
      <p class="flex justify-start mb-2 font-semibold">
        {{ question }}
      </p>
      <div v-if="answer == undefined" class="flex justify-start mb-2">
        <input
          v-model="input"
          class="
            border
            rounded-xl
            p-1
            focus:outline-none focus:shadow-outline
            mr-2
            flex-grow
          "
          type="text"
          placeholder="Type here"
        />
        <button
          v-if="!isRecording"
          class="
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-semibold
            p-2
            rounded-xl
            block
            mr-1
          "
          @click="record"
        >
          <img
            class=""
            src="~/assets/microphone.svg"
            width="14"
            alt="microphone"
          />
        </button>
        <button
          v-else
          class="
            bg-red-500
            hover:bg-red-700
            text-white
            font-semibold
            p-2
            rounded-xl
            block
            mr-1
          "
          @click="end"
        >
          Stop
        </button>

        <button
          class="
            bg-blue-500
            hover:bg-blue-700
            text-white
            font-semibold
            p-2
            rounded-xl
            block
          "
          @click="sendInput"
        >
          Send
        </button>
      </div>
      <div v-else class="mb-2">
        {{ answer }}
      </div>

      <div
        v-if="likes !== undefined"
        class="flex justify-end items-center mx-1"
      >
        <iconHeart class="w-4 mr-2" />
        <p class="font-bold">{{ likes }}</p>
      </div>
    </div>
  </main>
</template>

<script>
import { defineComponent, onMounted, ref } from '@nuxtjs/composition-api'
import iconHeart from '@/assets/iconHeart.vue'

export default defineComponent({
  name: 'QuestionCard',
  components: {
    iconHeart,
  },
  props: {
    id: {
      type: Number,
      required: true,
    },
    question: {
      type: String,
      required: true,
    },
    answer: {
      type: String,
      default: undefined,
    },
    likes: {
      type: Number,
      default: undefined,
    },
  },
  emits: ['refetch-text'],

  setup(props, context) {
    const isRecording = ref(false)
    const speech = ref()
    const input = ref('')

    onMounted(() => {
      // eslint-disable-next-line new-cap
      speech.value = new window.webkitSpeechRecognition()
      speech.value.lang = 'en-US'
      speech.value.interimResults = true

      speech.value.addEventListener('result', (e) => {
        const text = Array.from(e.results)
          .map((result) => result[0])
          .map((results) => results.transcript)
          .join('')
        input.value = text
      })
    })

    function record() {
      speech.value.start()
      isRecording.value = true
    }

    function end() {
      speech.value.stop()
      isRecording.value = false
    }

    const sendInput = async () => {
      const data = { id: props.id, answer: input.value }
      const response = await context.root.$axios.post(
        'https://chong-testbot.herokuapp.com/answer',
        data
      )

      context.emit('refetch-text', true)

      input.value = ''
      return response.data
    }

    return {
      input,
      record,
      end,
      sendInput,
      isRecording,
    }
  },
})
</script>
