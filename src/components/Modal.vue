<template>
    <v-dialog
      v-model="dialog.open"
      max-width="600px"
      @click:outside="closeDialog"
      scrollable
    >
      <v-card>
        <v-card-title v-if="contentLoaded()">
          <span class="headline">
            {{ dialog.content.post.title }}
          </span>
        </v-card-title>
        <v-skeleton-loader
          v-else
          class="skeleton"
          type="heading"
        ></v-skeleton-loader>
        <v-card-text v-if="contentLoaded()">
          {{ dialog.content.post.body }}
        </v-card-text>
        <v-skeleton-loader
          v-else
          class="skeleton"
          type="text @ 3"
        ></v-skeleton-loader>
      </v-card>
    </v-dialog>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    dialog: Object
  },
  methods: {
    closeDialog () {
      setTimeout(() => {
        this.$emit('update:dialog', {
          open: false,
          content: {
            post: undefined,
            user: undefined,
            comments: undefined
          }
        })
      }, 300)
    },
    contentLoaded () {
      return Boolean(this.dialog.content.comments)
    }
  }
}
</script>

<style scoped>
  .headline {
    text-transform: capitalize;
  }
  .skeleton {
    padding: 12px 24px 12px 24px
  }
  .v-card__text {
    text-transform: unset;
  }
</style>
