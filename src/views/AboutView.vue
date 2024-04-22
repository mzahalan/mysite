<template>
  <div class="home">
    <h1>Home</h1>
    <v-btn class="text-white bg-pink">buton</v-btn><br />
    <v-btn variant="flat" color="pink">click me</v-btn><br />
    <v-btn variant="text" color="pink">click me</v-btn><br />

    <v-btn variant="flat" class="text-white bg-pink d-none d-sm-flex">
      <template v-slot:prepend>
        <v-icon>mdi-email</v-icon>
      </template>
      <span>email me</span> </v-btn
    ><br />

    <v-btn size="small" variant="flat" class="text-white bg-pink">
      <span>email me</span>
      <template v-slot:append>
        <v-icon size="large">mdi-email</v-icon>
      </template> </v-btn
    ><br />
    <v-btn size="small" icon="mdi-star" color="purple"></v-btn>
  </div>
</template>
