<template>
  <v-sheet class="d-flex flex-column fill-height overflow-auto" max-height="90dvh">
    <div class="shrink">
      <v-btn v-if="!sock" @click="handleClick">Connect!</v-btn>
      <v-btn v-if="sock" @click="handleClose">Disconnect!</v-btn>
      <span v-if="spinner">Connecting...</span>
    </div>
    <div id="mudbox" class="overflow-auto flex-grow-1 pl-5">
      <div class="content ma-0 pa-0" v-for="msg in messages" :key="msg" v-html="msg"></div>
    </div>
    <div class="shrink">
      <v-text-field 
          ref="commandLine"
          v-model="message" 
          @keyup.enter="sendMessage" 
          @keyup.arrow-up="handleScroll(-1)"
          @keyup.arrow-down="handleScroll(1)"
          class="flex-1-1-100">
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
const commandLine = ref(null)
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
    })
  },
  { deep: true }
)

const history = []
const MAX_HISTORY = 50
let historyPointer = -1

const handleScroll = (inc) => {
  if(history.length > 0) {
    historyPointer = (historyPointer + inc) % history.length
    
    if(historyPointer < 0) {
      historyPointer = history.length -1
    }

    message.value = history[historyPointer]
    nextTick(() => {
      commandLine.value.select()
    })
  }
}

const addHistory = (msg) => {
  if(msg.trim() != '') {
    history.push(msg)
    if(history.length > MAX_HISTORY) {
      history.shift()
    }
    historyPointer = history.length - 1
  }
}
const sendMessage = () => {
  let msg = message.value
  addHistory(msg)
  if (sock.value) {
    sock.value.send(msg)
    commandLine.value.select()
  }
}
</script>

<style scoped>
.content {
  font-family: monospace;
}
</style>
