<template>
  <div>
    <button v-if="!sock" @click="handleClick">Connect!</button>
    <button v-if="sock" @click="handleClose">Disconnect!</button>
    <span v-if="spinner">Connecting...</span>
  </div>
  <div>
    <input v-model="message" @keyup.enter="sendMessage" />
    <button @click="sendMessage">Send</button>
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
    messages.value.push(ev.data)
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
