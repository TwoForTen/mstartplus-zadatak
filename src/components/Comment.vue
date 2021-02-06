<template>
    <v-list-item class="container">
        <v-list-item-content v-if="comment.id">
            <v-list-item-title class="comment">{{ comment.body }}</v-list-item-title>
            <v-list-item-subtitle class="comment">{{ comment.email }}</v-list-item-subtitle>
        </v-list-item-content>
        <v-skeleton-loader
          v-else
          class="skeleton"
          type="text @ 4"
        ></v-skeleton-loader>
    </v-list-item>
</template>

<script>
export default {
  name: 'Comment',
  props: {
    comment: Object
  }
}
</script>

<style scoped>
  .container {
      margin: .3em 0 .3em 0;
      padding: 0 1em 0 1em;
      width: 100%;
      border-left: 1px solid blue;
      border-radius: 0 !important;
  }
  .comment{
      white-space: normal;
      text-overflow: initial;
  }
</style>
