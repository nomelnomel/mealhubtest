<template>
  <div v-if="comments.length > 0" class="table">
    <div class="line">
      <div v-for="(cell, index) in cells" :key="index" @click="sort = cell">
        sort by {{ cell }}
      </div>
    </div>
    <nuxt-link v-for="{id, name, email} in pageData" :key="id" class="line" :to="'/' + id">
      <div>{{ id }}</div>
      <div>{{ name }}</div>
      <div>{{ email }}</div>
    </nuxt-link>
    <div class="pagination">
      <button :disabled="pageNumber === 0" @click="pageNumber--">
        &#8592;
      </button>
      <div class="counter">
        {{ pageNumber + 1 }}
      </div>
      <button :disabled="pageNumber >= pageCount-1" @click="pageNumber++">
        &#8594;
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
$border: 1px solid #bebefd;

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
    display: flex;
    align-items: center;

    button{
      border: none;
      background-color: white;
      color: #bebefd;
      font-size: 24px;

      &:disabled{
        color: #efefef;
      }
    }

    .counter {
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 50%;
      padding: 5px 10px;
      border: 1px solid #bebefd;
    }
  }
}
</style>
