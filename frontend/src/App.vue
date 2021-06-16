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
    fetch('backend/api/v1/users')
    .then(response => response.json())
    .then(json => {
        this.users = json
    })
  },
  methods: {
   async removeUser(id) {
        this.users = this.users.filter(t => t.id !== id)
          const requestOptions = {
        method: "DELETE"
      };
        const response = await fetch("http://localhost:5000/api/v1/delete/"+ id, requestOptions);

    },
    async addUser(user) {
        this.users.push(user)
        const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name: user.name, email: user.email })
  };

         const response = await fetch("http://localhost:5000/api/v1/add", requestOptions);
         const json = await response.json();
         this.users = json;
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
