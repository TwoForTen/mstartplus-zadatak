<template>
  <div>
    <SectionTitle :title="'Posts'" />
    <div v-if="loading" class="text-center pa-6">
      <v-progress-circular indeterminate color="primary"></v-progress-circular>
    </div>
    <div v-else>
      <Modal :dialog.sync="dialog" :request="request" />
      <template>
        <v-simple-table>
        <template v-slot:default>
          <tbody>
            <tr
              v-for="post in posts"
              :key="post.id"
            >
              <td @click="() => {
                  dialog.open = true
                  getPostComments(post.id, post, users[assignUserToPost(post)])
                }"
                class="py-3"
                style="cursor: pointer;"
                >
                <h3 class="primary--text font-weight-regular text-capitalize">
                  {{ post.title }}
                </h3>
                <span class="text--secondary">
                  by {{ users.length > 0 && users[assignUserToPost(post)].name }}
                </span>
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      </template>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import Modal from '../components/Modal'
import SectionTitle from '../components/SectionTitle'

export default {
  name: 'Home',
  components: {
    Modal,
    SectionTitle
  },
  data () {
    return {
      posts: [],
      users: [],
      loading: false,
      request: null,
      dialog: {
        open: false,
        loading: false,
        content: {
          post: undefined,
          user: undefined,
          comments: []
        }
      }
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData () {
      this.loading = true

      const fetchUsers = () =>
        axios.get('https://jsonplaceholder.typicode.com/users')

      const fetchPosts = () =>
        axios.get('https://jsonplaceholder.typicode.com/posts')

      Promise.all([fetchUsers(), fetchPosts()])
        .then(([users, posts]) => {
          this.users = users.data
          this.posts = posts.data
          this.loading = false
        }).catch(() => {
          this.loading = false
        })
    },
    assignUserToPost (post) {
      return this.users.findIndex((user) => user.id === post.userId)
    },
    getPostComments (postId, post, user) {
      this.dialog.loading = true
      this.request = axios.CancelToken.source()

      axios.get(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`, {
        cancelToken: this.request.token
      })
        .then(({ data }) => {
          this.dialog.content.comments = data
          this.dialog.content.post = post
          this.dialog.content.user = user
          this.dialog.loading = false
        }).catch(() => {
          this.dialog.loading = false
        })
    }
  }
}
</script>
