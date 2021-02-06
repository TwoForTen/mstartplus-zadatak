<template>
  <v-app>
    <v-app-bar hide-on-scroll fixed height="60" color="primary" class="appbar">
      <v-container style="display: flex; align-items: center;">
        <v-toolbar-title style="color: #fff;">Page title</v-toolbar-title>

        <v-spacer />
        <div class="links">
          <router-link to="/">Posts</router-link>
          <router-link to="/users">Users</router-link>
        </div>
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
    padding: 60px .5em .5em .5em !important;
  }
  .container {
    max-width: 1260px !important;
  }
  a {
    color: #fff !important;
    text-decoration: none;
    text-transform: uppercase;
  }
  .links > *:not(:last-child) {
    margin-right: 2em;
  }
</style>
