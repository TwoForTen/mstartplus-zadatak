<template>
    <v-dialog
      v-model="dialog.open"
      max-width="600px"
      @click:outside="closeDialog"
    >
      <v-card class="pa-5">
        <div class="d-flex justify-end">
          <v-btn @click="closeDialog" icon color="pink">
              <v-icon>mdi-close</v-icon>
          </v-btn>
        </div>
        <v-card-title v-if="!dialog.loading" class="pa-0 text-capitalize" style="word-break: normal;">
          {{ dialog.content.post && dialog.content.post.title }}
        </v-card-title>
        <v-skeleton-loader
          v-else
          class="py-2"
          type="heading"
        ></v-skeleton-loader>
        <v-card-text v-if="!dialog.loading" class="py-3 px-0 text--secondary" style="overflow: visible;">
          {{ dialog.content.post && dialog.content.post.body }}
        </v-card-text>
        <v-skeleton-loader
          v-else
          class="py-2"
          type="text @ 3"
        ></v-skeleton-loader>
        <h4 v-if="!dialog.loading" class="py-3">
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
