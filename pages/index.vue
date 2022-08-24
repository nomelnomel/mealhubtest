<template>
  <div v-if="comments.length > 0" class="table">
    <div class="line">
      <div v-for="(cell, index) in cells" :key="index" @click="sort = cell">
        {{ cell }}
      </div>
    </div>
    <nuxt-link v-for="{id, name, email} in pageData" :key="id" class="line" :to="'/' + id">
      <div>{{ id }}</div>
      <div>{{ name }}</div>
      <div>{{ email }}</div>
    </nuxt-link>
    <div class="pagination">
      <button :disabled="pageNumber === 0" @click="pageNumber--">
        prev
      </button>
      {{ pageNumber + 1 }}
      <button :disabled="pageNumber >= pageCount-1" @click="pageNumber++">
        next
      </button>
    </div>
  </div>
</template>

<script>
import { sortBy } from 'lodash'

export default {
  name: 'IndexPage',
  data () {
    return {
      comments: [],
      size: 10,
      pageNumber: 0,
      sort: 'id',
      cells: ['id', 'name', 'email']
    }
  },
  async fetch () {
    try {
      const res = await this.$axios.get('https://jsonplaceholder.typicode.com/comments')
      this.comments = res.data
    } catch (e) {
      console.log(e)
    }
  },
  computed: {
    pageCount () {
      return Math.floor(this.comments.length / this.size)
    },
    pageData () {
      const start = this.pageNumber * this.size
      const end = start + this.size
      return sortBy(this.comments.slice(start, end), this.sort)
    }
  }
}
</script>

<style scoped lang="scss">
$border: 1px solid #acacac;

.table {
  display: flex;
  flex-direction: column;

  .line{
    display: flex;
    border-bottom: $border;
    text-decoration: none;
    color: black;

    &:first-child{
      border-top: $border;
    }

    div{
      padding: 10px;
      border-right: $border;
      flex: 1 1 0;

      &:first-child{
        max-width: 100px;
        border-left: $border;
      }
    }
  }

  .pagination {
    align-self: center;
    padding: 10px;
  }
}
</style>
