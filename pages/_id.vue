<template>
  <div v-if="comment?.id">
    <div class="comment">
      <div><span>PostId</span>: {{ comment.postId }}</div>
      <div><span>Id</span>: {{ comment.id }}</div>
      <div><span>Name</span>: {{ comment.name }}</div>
      <div><span>Email</span>: {{ comment.email }}</div>
      <div><span>Body</span>: {{ comment.body }}</div>
    </div>
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

<style scoped lang="scss">
 .comment{
   display: flex;
   flex-direction: column;

   div span {
     font-weight: bold;
   }

 }
</style>
