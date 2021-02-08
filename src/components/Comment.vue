<template>
  <v-list-item class="my-4 px-4 container">
    <v-list-item-content>
      <v-list-item-title class="comment">{{ comment.body }}</v-list-item-title>
      <v-list-item-subtitle class="comment">{{ comment.email }}</v-list-item-subtitle>
    </v-list-item-content>
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
    border-left: 1px solid var(--v-primary-base);
  }
  .comment{
    white-space: normal;
    text-overflow: initial;
  }
</style>
