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
  <v-layout class="rounded rounded-md">
    <v-app-bar title="Application bar"></v-app-bar>

    <NavDrawer></NavDrawer>

    <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
      <RouterView></RouterView>
    </v-main>
  </v-layout>
</template>

<style scoped>

</style>
