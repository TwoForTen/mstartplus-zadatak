<template>
  <div>
    <Modal :dialog.sync="dialog" />
    <h2>Posts</h2>
    <template>
      <v-simple-table class="table" loading-text="LOADEING">
      <template v-slot:default>
        <tbody>
          <tr
            v-for="post in posts"
            :key="post.id"
          >
            <td @click.prevent="() => {
                dialog.open = true
                getPostComments(post.id, post, users[assignUserToPost(post)])
              }">
              <h3>
                {{ post.title }}
              </h3>
              <span>
                by {{ users.length > 0 && users[assignUserToPost(post)].name }}
              </span>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    </template>
  </div>
</template>

<script>
import axios from 'axios'
import Modal from '../components/Modal'

export default {
  name: 'Home',
  components: {
    Modal
  },
  data () {
    return {
      posts: [],
      users: [],
      dialog: {
        open: false,
        content: {
          post: undefined,
          user: undefined,
          comments: undefined
        }
      }
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
    },
    getPostComments (postId, post, user) {
      axios.get(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`)
        .then(({ data }) => {
          this.dialog.content.comments = data
          this.dialog.content.post = post
          this.dialog.content.user = user
        })
    }
  }
}
</script>

<style scoped>
  .table {
    width: 100%;
  }
  h2 {
    margin: 1.5em 0 .75em 0;
  }
  h3 {
    color: blue;
    font-weight: normal;
    text-transform: capitalize;
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
