<template>
    <div class="UserList">
       <ul>
       <UserItem
          v-for="(user, i) of users"
          v-bind:user="user"
          v-bind:index="i"
          v-on:remove-user="removeUser"
       />
       </ul>
    </div>
</template>

<script>
    import UserItem from '@/components/UserList/UserItem'
    export default {
        props: ['users'],
        components: {
        UserItem
        },
        methods: {
            removeUser(id) {
                this.$emit('remove-user', id)
            }
        }
    };
</script>

<style scoped>
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }
</style>