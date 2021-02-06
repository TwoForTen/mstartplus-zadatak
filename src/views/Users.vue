<template>
  <div>
    <SectionTitle :title="'Users'" />
    <v-data-table
      :headers="headers"
      :items="users"
      class="elevation-1"
      hide-default-footer
  >
    <template v-slot:[`item.avatar`]="{ item }">
      <div class="row_height">
        <v-avatar>
        <div class="img__container">
          <img
            alt="Avatar"
            :src="item.avatar"
          />
          </div>
        </v-avatar>
      </div>
    </template>
  </v-data-table>
  </div>
</template>

<script>
import axios from 'axios'
import SectionTitle from '../components/SectionTitle'

export default {
  name: 'Users',
  components: {
    SectionTitle
  },
  data () {
    return {
      users: [],
      headers: [
        { text: 'ID', align: 'left', value: 'id' },
        { text: 'Avatar', align: 'left', value: 'avatar', sortable: false },
        { text: 'Name', value: 'name' },
        { text: 'Email', value: 'email' }
      ]
    }
  },
  created () {
    this.fetchUsers()
  },
  methods: {
    fetchUsers (amount = 10) {
      axios.get(`https://jsonplaceholder.typicode.com/users?_limit=${amount}`)
        .then(({ data }) => {
          this.users = data.map((user, i) => {
            return {
              ...user,
              avatar: require(`../assets/${i}.png`)
            }
          })
        })
        .catch(() => {})
    }
  }
}
</script>

<style lang="scss" scoped>
  .img__container {
    object-fit: cover;
    padding: 1em 0 1em 0;
  }
  .row_height {
    padding: .75em 0 .75em 0;
  }
  img {
    height: 100%;
    width: 100%;
  }
</style>
