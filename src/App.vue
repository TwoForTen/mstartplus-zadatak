<template>
  <v-app>
    <v-app-bar hide-on-scroll fixed height="60" color="primary">
      <v-container class="d-flex align-center">
        <v-row class="d-flex align-center">
          <v-app-bar-nav-icon class="d-sm-none" style="color: #fff;" @click.prevent="drawer = !drawer"></v-app-bar-nav-icon>
          <v-toolbar-title style="color: #fff;">mStart Plus Zadatak</v-toolbar-title>
        </v-row>
        <v-spacer />
        <div class="links d-none d-sm-block">
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

    <v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
    >
      <v-list nav dense>
        <v-list-item to="/" exact active-class="primary--text">
          <v-list-item-title>Posts</v-list-item-title>
        </v-list-item>
        <v-list-item to="/users" exact active-class="primary--text">
          <v-list-item-title>Users</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    drawer: false
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
