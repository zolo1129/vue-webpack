<template lang="pug">
  .app-todo
    input.app-input(
      type="text"
      placeholder="待办事项"
      autofocus="autofocus"
      @keyup.enter="addTodo"
    )
    Item(
      :todo="todo"
      v-for="todo in filteredTodos"
      :key="todo.id"
      @del='deleteTodo'
    )
    Tabs(
      :filter="filter"
      :todos="todos"
      @tab="filterTodo"
      @clearAllCompleted="clearAllComplete"
    )
</template>

<script>
import Item from './item.vue'
import Tabs from './tabs.vue'

let id = 0

export default {
  data() {
    return {
      todos: [],
      filter: 'all'
    }
  },
  components: {
    Item,
    Tabs
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos
      }
      const complete = this.filter === 'completed'
      return this.todos.filter(todo => complete === todo.complete)
    }
  },
  methods: {
    addTodo(e) {
      this.todos.unshift({
        id: id++,
        content: e.target.value.trim(),
        complete: false
      })
      e.target.value = ''
    },
    deleteTodo(id) {
      this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1)
    },
    filterTodo(state) {
      this.filter = state
    },
    clearAllComplete() {
      this.todos = this.todos.filter(todo => todo.complete === false)
    }
  }
}
</script>

<style lang="sass" scoped>
.app-todo
  width: 700px
  margin: 0 auto 40px
  box-shadow: 0 0 5px #666
.app-input
  position: relative
  margin: 0
  width: 100%
  height: 40px
  font-size: 20px
  border: none
  padding: 0 24px 0 40px
  box-sizing: border-box
.app-input::-webkit-input-placeholder
  color: #ccc
</style>


