<template>
  <div>
    <h2>Todo applications</h2>
    <router-link to="/"> Home </router-link>
    <hr />
    <AddTodo @add-todo="addTodo" />
    <hr />
    <select v-model="filter">
      <option value="all">ALL</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not Completed</option>
    </select>
    <Loader v-if="loading" />
    <TodoList
      v-else-if="todos.length"
      :todos="filteredTodes"
      @remove-todo="removeTodo"
    />
    <p v-else>No todos</p>
  </div>
</template>


<script>
import TodoList from "../components/TodoList";
import AddTodo from "../components/AddTodo";
import Loader from "../components/Loader";
export default {
  name: "App",
  data() {
    return {
      todos: [],
      loading: true,
      filter: "all",
    };
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=3")
      .then((response) => response.json())
      .then((json) => {
        this.todos = json;
        this.loading = false;
      });
  },
  //   watch: {
  //     filter(value) {
  //       console.log(value);
  //     },
  //   },

  computed: {
    filteredTodes() {
      if (this.filter === "all") {
        return this.todos;
      }

      if (this.filter === "completed") {
        return this.todos.filter((t) => t.completed);
      }

      if (this.filter === "not-completed") {
        return this.todos.filter((t) => !t.completed);
      }
    },
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((t) => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    },
  },
  components: { TodoList, AddTodo, Loader },
};
</script>