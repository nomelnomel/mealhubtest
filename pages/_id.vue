<template>
  <div v-if="comment?.id">
    {{ comment }}
  </div>
</template>

<script>
export default {
  name: 'CommentId',
  data () {
    return {
      comment: {}
    }
  },
  async fetch () {
    try {
      const res = await this.$axios.get(`https://jsonplaceholder.typicode.com/comments/${this.$route.params.id}`)
      this.comment = res.data
    } catch (e) {
      console.log(e)
    }
  }
}
</script>

<style scoped>

</style>
