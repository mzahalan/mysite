<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar title="Zahalan.com">
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <v-spacer></v-spacer>
      <v-btn icon="mdi-theme-light-dark" @click="toggleTheme"></v-btn>
    </v-app-bar>

    <NavDrawer v-model="drawer"></NavDrawer>

    <v-main class="d-flex align-center justify-center" style="min-height: 300px">
      <RouterView></RouterView>
    </v-main>
  </v-layout>
</template>

<script setup>
import NavDrawer from './components/NavDrawer.vue'
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
