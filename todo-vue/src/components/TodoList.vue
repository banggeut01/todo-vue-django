<template>
  <div class="todo-list">
      <h2>투두투두투</h2>
      <ul>
          <li 
          v-for="todo in todos" 
          :key="todo.id"
          >
              {{ todo.title }}
          </li>
      </ul>
  </div>
</template>

<script>
export default {
    name: 'TodoList',
    props: {
        //
        todos: {
            type: Array,
            required: true
        }
    }
}
</script>

<style>

</style>