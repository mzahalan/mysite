<script setup>
import NavDrawer from './components/NavDrawer.vue'
import {ref} from 'vue'
import {useTheme} from 'vuetify'

const theme = useTheme()
const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? "light" : "dark"
}

const drawer = ref(false)

document.title = 'Zahalan.com'
</script>

<template>
  <v-app>      
    <v-app-bar
      image="https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg"
      dark
      prominent
      app
    >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-app-bar-title>Vuetify</v-app-bar-title>
      <v-spacer></v-spacer>
      <v-btn icon="mdi-theme-light-dark" @click="toggleTheme"></v-btn>
    </v-app-bar>
    <NavDrawer v-model="drawer"/>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<style scoped>

</style>
