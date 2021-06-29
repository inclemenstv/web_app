<template>
<li>
   <span>
   <strong>{{index+1}}</strong>
   {{user.user_name}}
   {{user.email}}
   <button class="rm"
        v-on:click="$emit('remove-user', user.id)"
   >&times;</button>
   </span>
</li>
</template>

<script>
export default {
    props: {
        user: {
        type: Object,
        required: true
        },
        index: Number
    },

}
</script>

<style scoped>
  li {
    line-height: 3;
    border: 2px solid #ccc;
    display: flex;
    justify-content: space-between;
    padding: .20rem 2rem;
    margin-bottom: 1rem;
  }

  .rm {
    background: red;
    color: #fff;
    border-radius: 50%;
    font-weight: bold;
  }

  input {
    margin-right: 1rem;
  }

  .done {
    text-decoration: line-through;
  }

</style>