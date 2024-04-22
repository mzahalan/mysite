<template>
  <v-app>
    <v-app-bar title="Zahalan.com">
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <template v-slot:append>
        <v-btn icon="mdi-theme-light-dark" @click="toggleTheme"></v-btn>
      </template>
    </v-app-bar>
    <v-navigation-drawer v-model="drawer">
      <v-list-item><router-link :to="{ name: 'home' }">Home</router-link></v-list-item>
      <v-list-item><router-link :to="{ name: 'about' }">About</router-link></v-list-item>
      <v-list-item><router-link :to="{ name: 'mudconnect' }">Mud</router-link></v-list-item>
    </v-navigation-drawer>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()
const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
}

const drawer = ref(false)

document.title = 'Zahalan.com'
</script>

<style scoped></style>
