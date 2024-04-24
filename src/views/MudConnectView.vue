<template>
  <v-sheet class="d-flex flex-column fill-height overflow-auto" max-height="90dvh">
    <div class="shrink">
      <v-btn v-if="!sock" @click="handleClick">Connect!</v-btn>
      <v-btn v-if="sock" @click="handleClose">Disconnect!</v-btn>
      <span v-if="spinner">Connecting...</span>
    </div>
    <div id="mudbox" class="overflow-auto flex-grow-1">
      <template v-for="msg in messages" :key="msg">
        <pre>
      {{ msg }}
    </pre
        >
        <br />
      </template>
    </div>
    <div class="shrink">
      <v-text-field v-model="message" @keyup.enter="sendMessage" class="flex-1-1-100">
        <template v-slot:append-inner>
          <button @click="sendMessage"><v-icon>mdi-send</v-icon></button>
        </template>
      </v-text-field>
    </div>
  </v-sheet>
</template>

<script setup>
import { nextTick, ref, watch } from 'vue'
const spinner = ref(false)
const sock = ref(null)
const messages = ref([])
const message = ref('')

const handleClick = () => {
  spinner.value = true
  sock.value = new WebSocket('wss://socket.zahalan.com')
  sock.value.onmessage = (ev) => {
    messages.value.push(ev.data + '\n')
  }
  spinner.value = false
}

const handleClose = () => {
  sock.value.close()
  sock.value = ''
}

watch(
  () => messages.value,
  async (msgs) => {
    nextTick(() => {
      let mudbox = document.getElementById('mudbox')
      mudbox.scrollTop = mudbox.scrollHeight
      console.log('updated scroll')
    })
  },
  { deep: true }
)

const sendMessage = () => {
  let msg = message.value
  console.log(`Sending Message: ${msg}`)
  if (sock.value) {
    sock.value.send(msg)
    message.value = ''
  }
}
</script>

<style scoped></style>
