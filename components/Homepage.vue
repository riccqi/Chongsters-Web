<template>
    <div class="m-2">
        <div class="font-bold text-xl pb-8">
            Welcome to the Chongsters Web
        </div>
        <div>
            <form>
                <label class="block text-gray-700 m-1 font-bold" for="input">Input</label>
                <input id="input" v-model="input" class="mb-4 shadow appearance-none border rounded p-2 focus:outline-none focus:shadow-outline" type="text" placeholder="Type here">
            </form>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold p-2 rounded block" @click="sendInput">
                Enter
            </button>
        </div>
        <p class="text-center text-gray-500 text-xs">
            &copy;2021 Chongsters. All rights reserved.
        </p>
    </div>
</template>

<script>
import { defineComponent, ref } from '@nuxtjs/composition-api'

export default defineComponent({
    name: "Homepage",
    setup() {
        const input = ref('')

        const sendInput = () => {
            window.alert("The input is" + input.value)
        }
        
        return {
            input,
            sendInput
        }
    },
})
</script>
