<template>
  <v-sheet class="d-flex flex-column fill-height overflow-auto" max-height="90dvh">
    <div class="shrink">
      <v-toolbar>
        <v-toolbar-title>MudConnect</v-toolbar-title>
        <v-text-field
          label="Character"
          type="text"
          variant="outlined"
          hide-details
          v-model="character"
        ></v-text-field>
        
      <v-text-field
          label="Password"
          type="password"
          variant="outlined"
          hide-details
          v-model="password"
      ></v-text-field>
        <v-btn v-if="!sock" @click="handleClick">Connect!</v-btn>
        <v-btn v-if="sock" @click="handleClose">Disconnect!</v-btn>
      </v-toolbar>
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
          <template v-slot:prepend-inner><v-icon>mdi-code-greater-than</v-icon></template>
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
const password = ref('')
const character = ref('')


const sendCharName = () => {
  let outMsg = {
      message: character.value
  }
  sock.value.send(JSON.stringify(outMsg))
}

const sendPassword = () => {
  let outMsg = {
      message: password.value
  }
  sock.value.send(JSON.stringify(outMsg))
}

const receiveData = (ev) => {
  const PROMPT_PATTERN_CHARACTER = /By what name do you wish to be known\?$/
  const PROMPT_PATTERN_PASSWORD = /^Password:$/
  let msg = JSON.parse(ev.data)

  messages.value.push(msg.html + '\n')
  if(PROMPT_PATTERN_CHARACTER.test(msg.text.trim())) {
    sendCharName()
    messages.value.push(`<div class='text-blue-darken-1'>${character.value}</div>`)
  } else if (PROMPT_PATTERN_PASSWORD.test(msg.text.trim())) {
    sendPassword()
    messages.value.push("<div class='text-blue-darken-1'>PASSWORD SENT</div>")
  }

}

const handleClick = () => {
  spinner.value = true
  sock.value = new WebSocket('wss://socket.zahalan.com')
  //sock.value = new WebSocket('ws://localhost:8081')
  sock.value.onmessage = receiveData
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
    let outMsg = {
      message: msg
    }
    sock.value.send(JSON.stringify(outMsg))
    commandLine.value.select()
  }
}
</script>

<style scoped>
.content {
  font-family: monospace;
}
</style>
