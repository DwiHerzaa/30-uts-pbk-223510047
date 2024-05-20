<template>
  <div class="card mb-3">
    <div class="card-body">
      <h5 class="card-title">{{ post.title }}</h5>
      <p class="card-text">{{ post.body }}</p>
      <p class="card-text">User: {{ getUserById(post.userId).name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['post', 'users'],
  methods: {
    getUserById(id) {
      return this.users.find(user => user.id === id) || { name: 'Unknown' };
    }
  }
}
</script>
