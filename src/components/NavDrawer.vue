<template>
    <v-navigation-drawer>
        <v-list-item title="My Application" subtitle="Vuetify"></v-list-item>
        <v-divider></v-divider>
        <v-list density="compact">
            <v-list-item><router-link :to="{name: 'home'}">Home</router-link></v-list-item>
            <v-list-item><router-link :to="{name: 'about'}">About</router-link></v-list-item>
        </v-list>
    </v-navigation-drawer>
</template>

<script>
export default {
    setup () {
        

        return {}
    }
}
</script>
