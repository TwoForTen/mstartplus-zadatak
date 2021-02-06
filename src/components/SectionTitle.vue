<template>
    <h2>{{ title }}</h2>
</template>

<script>
export default {
  name: 'SectionTitle',
  props: {
    title: String
  }
}
</script>

<style scoped>
    h2 {
      margin: .5em 0 .75em 0;
      color: rgb(114, 114, 114);
    }
</style>
