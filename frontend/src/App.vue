<template>
  <div id="app">
  <h1>Users</h1>
  <AddUser
    @add-user="addUser"
  />
  <hr>
  <UserList
  v-bind:users="users"
  @remove-user="removeUser"
  />
  </div>
</template>

<script>
import UserList from '@/components/UserList/UserList'
import AddUser from '@/components/UserList/AddUser'
export default {
  name: 'App',
  data() {
  return {
    users: []
    }
  },
  mounted() {

  this.fetchUsers()
    },
  methods: {
fetchUsers() {
  fetch('http://192.168.50.11:30500/api/v1/users')
    .then(response => response.json())
    .then(json => {
                 console.log(json);
        this.users = json;
                 console.log(this.users);
    })
  },
   async removeUser(id) {
        this.users = this.users.filter(t => t.id !== id)
          const requestOptions = {
        method: "DELETE"
      };
        const response = await fetch("http://192.168.50.11:30500/api/v1/delete/"+ id, requestOptions);

    },
    async addUser(user) {

        const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name: user.name, email: user.email })
  };

         const response = await fetch("http://192.168.50.11:30500/api/v1/add", requestOptions);
         const json = await response.json();
         this.fetchUsers()
    }
  },

  components: {
    UserList, AddUser
  }
}
</script>

<style>
#app {
  font-family: Arial, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
