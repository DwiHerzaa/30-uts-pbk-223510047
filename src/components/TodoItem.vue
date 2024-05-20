<template>
    <li class="list-group-item d-flex justify-content-between align-items-center" :class="{ 'completed': item.completed }">
      <slot name="todo-text" v-if="!item.completed">{{ item.text }}</slot>
      <slot name="completed-text" v-else style="text-decoration: line-through;">{{ item.text }}</slot>
      <div>
        <slot name="remove-button">
          <button class="btn btn-danger btn-sm mr-2" @click="$emit('remove')">X</button>
        </slot>
        <slot name="toggle-button">
          <button class="btn btn-secondary btn-sm" @click="$emit('toggle')">{{ item.completed ? 'Undone' : 'Done' }}</button>
        </slot>
      </div>
    </li>
  </template>
  
  <script>
  export default {
    props: ['item']
  }
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
  }
  </style>
  