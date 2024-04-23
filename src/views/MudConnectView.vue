<template>
  <div>
    <v-btn v-if="!sock" @click="handleClick">Connect!</v-btn>
    <v-btn v-if="sock" @click="handleClose">Disconnect!</v-btn>
    <span v-if="spinner">Connecting...</span>
  </div>
  <v-textarea v-model="messages">{{ messages }} </v-textarea>
  <div>
    <v-text-field v-model="message" @keyup.enter="sendMessage">
      <template v-slot:append-inner>
        <button @click="sendMessage">Send</button>
      </template>
    </v-text-field>
    <ul>
      <li v-for="msg in messages" :key="msg">{{ msg }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const spinner = ref(false)
const sock = ref(null)
const messages = ref([])
const message = ref('')

const handleClick = () => {
  spinner.value = true
  sock.value = new WebSocket('wss://socket.zahalan.com')
  sock.value.onmessage = (ev) => {
    console.log(ev)
    messages.value.push(ev.data + '\n')
  }
  console.log(sock.value)
  spinner.value = false
}

const handleClose = () => {
  sock.value.close()
  sock.value = ''
}

const sendMessage = () => {
  let msg = message.value
  console.log(`Sending Message: ${msg}`)
  if (msg.trim() !== '' && sock.value) {
    sock.value.send(msg)
    message.value = ''
  }
}
</script>

<style scoped></style>
