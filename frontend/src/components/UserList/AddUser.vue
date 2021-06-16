<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model="name">
    <input type="text" v-model="email">
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
      if (this.name.trim()) {
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