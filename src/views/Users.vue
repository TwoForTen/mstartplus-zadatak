<template>
  <div>
    <SectionTitle :title="'Users'" />
    <v-data-table
      :headers="headers"
      :items="users"
      class="elevation-1"
      hide-default-footer
      :loading="loading"
      loading-text="Fetching users... Please wait."
  >
    <template v-slot:[`item.avatar`]="{ item }">
      <div class="py-2">
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
    <template v-slot:top>
      <v-container>
        <v-select
          class="pa-1 font-weight-bold"
          :items="[1,2,3,4,5,6,7,8,9,10]"
          label="Users Amount"
          v-model="amount"
          @change="fetchUsers(amount)"
        ></v-select>
      </v-container>
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
      amount: 10,
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Avatar', value: 'avatar', sortable: false },
        { text: 'Name', value: 'name' },
        { text: 'Email', value: 'email' }
      ],
      loading: false
    }
  },
  created () {
    this.fetchUsers(this.amount)
  },
  methods: {
    fetchUsers (amount) {
      this.loading = true
      axios.get(`https://jsonplaceholder.typicode.com/users?_limit=${amount}`)
        .then(({ data }) => {
          this.users = data.map((user, i) => {
            return {
              ...user,
              avatar: require(`../assets/${i}.png`)
            }
          })
          this.loading = false
        })
        .catch(() => {
          this.loading = false
        })
    }
  }
}
</script>

<style lang="scss" scoped>
  .img__container {
    object-fit: cover;
  }
  img {
    height: 100%;
    width: 100%;
  }
</style>
