<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model="name" placeholder="New name">
    <input :placeholder="Email" type="text" v-model="email" placeholder="New email">
    <button type="submit">Create</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      email: ''
    }
  },
  methods: {
    onSubmit() {
      console.log('Submit', this.name, this.email)
      if (this.name.trim() && this.email.trim()) {
      const newUser = {
        id: Date.now(),
        name: this.name,
        email: this.email
      }

        this.$emit('add-user', newUser)
        this.name=''
        this.email=''
      }
    }
  }
}
</script>

<style scoped>
  form {
    display: flex;
  }

  input {
    width: 400px;
  }
</style>