<template>
  <v-app dark>
    <v-main class="bg-main">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>
