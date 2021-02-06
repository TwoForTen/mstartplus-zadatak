<template>
  <v-app>
    <v-app-bar hide-on-scroll fixed color="blue" height="60">
      <v-container style="display: flex; align-items: center;">
        <v-toolbar-title>Page title</v-toolbar-title>

        <v-spacer />
        <router-link to="/">Posts</router-link>
        <router-link to="/about">Users</router-link>
      </v-container>
    </v-app-bar>
    <v-main>
      <v-container>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    //
  })
}
</script>

<style>
  main {
    padding: 60px 1em 1em 1em !important;
  }
  .container {
    max-width: 1260px !important;
  }
</style>
