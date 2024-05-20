<template>
  <div class="container">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO & POST APP</h5>
        <div class="row">
          <div class="col-12">
            <ul class="nav nav-tabs">
              <li class="nav-item">
                <button class="nav-link" :class="{ 'active': activeTab === 'todos' }" @click="activeTab = 'todos'">Todos</button>
              </li>
              <li class="nav-item">
                <button class="nav-link" :class="{ 'active': activeTab === 'posts' }" @click="activeTab = 'posts'">Posts</button>
              </li>
            </ul>
          </div>
        </div>

        <div v-show="activeTab === 'todos'" class="mt-3">
          <TodoList />
        </div>

        <div v-show="activeTab === 'posts'" class="mt-3">
          <PostList />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'
import PostList from './components/PostList.vue'

export default {
  components: {
    TodoList,
    PostList
  },
  data() {
    return {
      activeTab: 'todos'
    }
  }
}
</script>

<style scoped>
.nav-tabs .nav-link {
  color: #333;
  font-weight: bold;
}
.nav-tabs .nav-link.active {
  background-color: #007bff;
  color: #fff;
}
</style>
