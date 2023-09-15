<template>
  <main class="home">
    <div class="home-wrapper">
      <h1 class="header-text">Todo App</h1>
      <div class="input-wrapper">
        <!-- INPUT ELEMENT -->
        <TodoInput @add="addNewTodo($event)" />

        <!-- FILTER -->
        <TodoFilter :filterOptions="filterOptions" :filter="filter" @addFilters="selectedFilter($event)" />
      </div>

      <!-- CARDS -->
      <ul v-if="filteredTodos.length">
        <li v-for="(todo, index) in filteredTodos" v-bind:key="index">
          <TodoCard :todo="todo" @delete="deleteTodo($event)" @updateStatus="updateTodoStatus($event)" />
        </li>
      </ul>
      <h3 v-else class="no-todos">Nothing to see here, either create a new todo or go to bed</h3>
    </div>
  </main>
</template>

<script>
import TodoFilter from '../../components/todo-filter/index.vue'
import TodoCard from '../../components/todo-card/index.vue'
import TodoInput from '../../components/todo-input/index.vue'

import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'HomeView',
  components: {
    TodoInput,
    TodoCard,
    TodoFilter
  },
  data() {
    return {
      filter: 'all',
      todos: [],
      filterOptions: ['all', 'completed', 'uncompleted'],
    }
  },

  methods: {
    addNewTodo: function (todo) {
      this.todos.push({ id: uuidv4(), title: todo, status: 'uncompleted' })
    },

    deleteTodo: function (todo) {
      this.todos = this.todos.filter((item) => { return item.id !== todo.id })
    },
    updateTodoStatus: function (todo) {
      const todoIndex = this.todos.findIndex((item) => item.id === todo.id)
      this.todos[todoIndex].status = todo.status === "completed" ? "uncompleted" : "completed";
    },
    selectedFilter: function (e) {
      this.filter = e
    }
  },
  computed: {
    filteredTodos: function () {
      console.log()
      return this.todos.filter((item) => {
        if (this.filter === 'all') {
          return item
        }
        else {
          return item.status === this.filter;
        }

      });
    }
  }
}
</script>


<style lang="css" scoped>
@import './index.css';
</style>

