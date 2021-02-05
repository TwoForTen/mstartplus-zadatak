<template>
  <v-app>
    <v-app-bar hide-on-scroll fixed color="blue" height="60">
    </v-app-bar>
    <v-main>
      <Home />
    </v-main>
  </v-app>
</template>

<script>
import Home from './components/Home'

export default {
  name: 'App',

  components: {
    Home
  },

  data: () => ({
    //
  })
}
</script>

<style>
  main {
    padding: 60px 1em 1em 1em !important;
  }
</style>
