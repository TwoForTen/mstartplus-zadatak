<template>
<div class="container">
  <v-simple-table class="table">
    <template v-slot:default>
      <tbody>
        <tr
          v-for="post in posts"
          :key="post.id"
        >
          <td>
            <h3>
              {{ post.title.split(' ')
              .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
              .join(' ') }}
            </h3>
            <span>
              by {{ users.length > 0 && users[assignUserToPost(post)].name }}
            </span>
          </td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data () {
    return {
      posts: [],
      users: []
    }
  },
  created () {
    this.fetchData()
  },

  methods: {
    fetchData () {
      const fetchUsers = () =>
        axios.get('https://jsonplaceholder.typicode.com/users')

      const fetchPosts = () =>
        axios.get('https://jsonplaceholder.typicode.com/posts')

      Promise.all([fetchUsers(), fetchPosts()])
        .then(([users, posts]) => {
          this.users = users.data
          this.posts = posts.data
        })
    },
    assignUserToPost (post) {
      return this.users.findIndex((user) => user.id === post.userId)
    }
  }
}
</script>

<style scoped>
  .container {
    padding: 2em;
    margin: 0 auto;
    max-width: 1260px;
  }
  .table {
    width: 100%;
  }
  h3 {
    color: blue;
    font-weight: normal;
  }
  span {
    color: rgba(0,0,0,0.5)
  }
  td {
    padding-top: 1em !important;
    padding-bottom: 1em !important;
    cursor: pointer;
  }
</style>
