<template>
    <div>
      <div class="row">
        <div class="col-10">
          <input v-model="todo" type="text" class="form-control" placeholder="Add new todo..." @keyup.enter="add" />
        </div>
        <div class="col-2">
          <button class="btn btn-success btn-block" @click="add">ADD</button>
        </div>
      </div>
      <small class="text-muted">Total TODO : {{ todos.length }}</small>
      <ul class="list-group mt-3">
        <TodoItem v-for="(item, index) in todos" :key="index" :item="item" @remove="remove(index)" @toggle="toggleCompletion(index)" />
      </ul>
    </div>
  </template>
  
  <script>
  import TodoItem from './TodoItem.vue'
  
  export default {
    components: {
      TodoItem
    },
    data() {
      return {
        todo: '',
        todos: []
      }
    },
    methods: {
      add() {
        if (this.todo.trim() !== '') {
          this.todos.push({ text: this.todo, completed: false });
          this.todo = '';
        }
      },
      remove(index) {
        this.todos.splice(index, 1);
      },
      toggleCompletion(index) {
        this.todos[index].completed = !this.todos[index].completed;
      }
    }
  }
  </script>
  