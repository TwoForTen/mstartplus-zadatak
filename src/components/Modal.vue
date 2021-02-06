<template>
    <v-dialog
      v-model="dialog.open"
      max-width="600px"
      @click:outside="closeDialog"
    >
      <v-card class="dialog">
        <div class="button__close">
          <v-btn @click="closeDialog" icon color="pink">
              <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
        <v-card-title v-if="!dialog.loading">
          {{ dialog.content.post.title }}
        </v-card-title>
        <v-skeleton-loader
          v-else
          class="skeleton"
          type="heading"
        ></v-skeleton-loader>
        <v-card-text v-if="!dialog.loading">
          {{ dialog.content.post.body }}
        </v-card-text>
        <v-skeleton-loader
          v-else
          class="skeleton"
          type="text @ 3"
        ></v-skeleton-loader>
        <h4 v-if="!dialog.loading">
          Comments ({{ dialog.content.comments.length }})
        </h4>
        <div v-for="comment in dialog.content.comments" :key="comment.id">
          <Comment :comment="comment" />
        </div>
      </v-card>
    </v-dialog>
</template>

<script>
import Comment from './Comment'

export default {
  name: 'Modal',
  props: {
    dialog: Object
  },
  components: {
    Comment
  },
  methods: {
    closeDialog () {
      this.$emit('update:dialog', {
        ...this.dialog,
        open: false
      })
      setTimeout(() => {
        this.$emit('update:dialog', {
          ...this.dialog,
          content: {
            user: undefined,
            post: undefined,
            comments: []
          }
        })
      }, 300)
    }
  }
}
</script>

<style scoped>
  .dialog {
    padding: 1.5em
  }
  .button__close {
    display: flex;
    justify-content: flex-end;
  }
  .skeleton {
    padding: .3em 0 .3em 0;
  }
  .v-card__title {
    padding: 0 !important;
    text-transform: capitalize;
    word-break: normal;
  }
  .v-card__text {
    padding: .5em 0 .5em 0 !important;
    color: rgb(41, 41, 41) !important;
    overflow: visible !important;
  }
  h4 {
    padding: .75em 0 .75em 0 !important;
  }
</style>
