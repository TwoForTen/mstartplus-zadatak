<template>
    <v-dialog
      v-model="dialogOpen"
      max-width="600px"
      @click:outside="closeDialog"
    >
      <v-card>
        <v-card-title>
          <span class="headline">Use Google's location service?</span>
        </v-card-title>
        <v-card-text>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Expedita doloremque accusantium dolores sunt delectus aliquid corporis accusamus sapiente tempora impedit!
        </v-card-text>
      </v-card>
    </v-dialog>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    dialogOpen: Boolean
  },
  methods: {
    closeDialog () {
      this.$emit('update:dialogOpen', false)
    }
  }
}
</script>

<style scoped>

</style>
