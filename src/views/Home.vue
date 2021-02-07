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
              v-for="post in displayedPosts"
              :key="post.id"
            >
              <td @click="() => {
                  dialog.open = true
                  getPostComments(post.id, post)
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
      <v-pagination v-model="page" @input="paginate()" :length="Math.ceil(allPosts.length / POSTS_PER_PAGE)"></v-pagination>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import Modal from '../components/Modal'
import SectionTitle from '../components/SectionTitle'

const POSTS_PER_PAGE = 10

export default {
  name: 'Home',
  components: {
    Modal,
    SectionTitle
  },
  data () {
    return {
      allPosts: [],
      displayedPosts: [],
      users: [],
      loading: false,
      request: null,
      page: 1,
      dialog: {
        open: false,
        loading: false,
        content: {
          post: undefined,
          comments: []
        }
      }
    }
  },
  created () {
    this.fetchData()
    this.POSTS_PER_PAGE = POSTS_PER_PAGE
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
          this.allPosts = posts.data
          this.loading = false
          this.paginate()
        }).catch(() => {
          this.loading = false
        })
    },
    assignUserToPost (post) {
      return this.users.findIndex((user) => user.id === post.userId)
    },
    paginate () {
      this.displayedPosts = this.allPosts.slice((this.page - 1) * this.POSTS_PER_PAGE, ((this.page - 1) * this.POSTS_PER_PAGE) + this.POSTS_PER_PAGE)
    },
    getPostComments (postId, post) {
      this.dialog.loading = true
      this.request = axios.CancelToken.source()

      axios.get(`https://jsonplaceholder.typicode.com/comments?postId=${postId}`, {
        cancelToken: this.request.token
      })
        .then(({ data }) => {
          this.dialog.content.comments = data
          this.dialog.content.post = post
          this.dialog.loading = false
        }).catch(() => {
          this.dialog.loading = false
        })
    }
  }
}
</script>
