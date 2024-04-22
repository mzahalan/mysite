<template>
  <v-app>
    <v-app-bar title="Zahalan.com" prominent app>
      <template v-slot:prepend>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      </template>
      <v-spacer></v-spacer>
      <v-btn icon="mdi-theme-light-dark" @click="toggleTheme"></v-btn>
    </v-app-bar>
    <NavDrawer v-model="drawer"></NavDrawer>
    <v-main>
      <RouterView></RouterView>
    </v-main>
  </v-app>
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
