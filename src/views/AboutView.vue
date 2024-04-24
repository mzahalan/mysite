<template>
  <v-container>
    <v-row class="fill-height pb-14" align="end">
      <v-col>
        <div v-for="(item, index) in chat" :key="index" :class="['d-flex flex-row my-2']">
          <span>{{ item.from[0] }}</span>
          <span>{{ item.msg }}</span>
        </div>
      </v-col>
    </v-row>
  </v-container>
  <v-footer fixed>
    <v-container class="ma-0 pa-0">
      <v-row no-gutters>
        <v-col>
          <div class="d-flex flex-row align-center">
            <v-text-field
              v-model="message"
              placeholder="Type Something"
              @keypress.enter="send"
            ></v-text-field>
            <v-btn icon class="ml-4" @click="send"><v-icon>mdi-send</v-icon></v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </v-footer>
</template>

<script setup>
import { ref } from 'vue'

const chat = ref([])
const message = ref(null)

const send = () => {
  chat.value.push({ from: 'user', msg: message.value })
  message.value = null
  addReply()
}

const addReply = () => {
  chat.value.push({
    from: 'sushant',
    msg: 'Hmm'
  })
}
</script>
