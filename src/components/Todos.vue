<template>
  <div class="container">
    <h2>My Check List</h2>
    <input
      type="text"
      class="todo-input"
      placeholder="Please input an item"
      :value="newTodoTitle"
      v-on:keyup.enter="addTodo"
    />
    <TodoItem
      v-on:deleteItem="handleDeleteTodo"
      v-on:toggleComplete="handleToggleComplete"
      v-for="(todo,index) in filteredTodos"
      :key="index"
      :todo.sync="todo"
    />
    <div class="inner-container">
      <div>
        <label>
          <input class="inner-container-input" type="checkbox" v-on:change="checkAllTodos" />
          Check All
        </label>
      </div>
      <div>{{todosRemaining}} items left!!</div>
    </div>
    <div class="inner-container">
      <div>
        <button :class="{active: (currentFilter ==='all')}" v-on:click="updateFilter('all')">All</button>
        <button
          :class="{active: (currentFilter ==='active')}"
          v-on:click="updateFilter('active')"
        >Active</button>
        <button
          :class="{active: (currentFilter ==='completed')}"
          v-on:click="updateFilter('completed')"
        >Completed</button>
      </div>
      <dir>
        <button @click="clearCompleted">Clear Completed</button>
      </dir>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
export default {
  name: "todos",
  components: { TodoItem },
  data() {
    return {
      newTodoTitle: "",
      currentFilter: "all",
      nextId: 4,
      todos: [
        {
          id: 1,
          title: "My First Todo",
          completed: false
        },
        {
          id: 2,
          title: "My Second Todo",
          completed: false
        },
        {
          id: 3,
          title: "My Third Todo",
          completed: false
        }
      ]
    };
  },
  computed: {
    todosRemaining() {
      return this.filteredTodos.filter(todo => !todo.completed).length;
    },
    filteredTodos() {
      return this.currentFilter === "all"
        ? this.todos
        : this.currentFilter === "completed"
        ? this.todos.filter(todo => todo.completed)
        : this.todos.filter(todo => !todo.completed);
    }
  },
  methods: {
    addTodo(event) {
      this.newTodoTitle = event.target.value;
      this.todos.push({
        id: this.nextId,
        completed: false,
        title: this.newTodoTitle
      });
      this.nextId = this.nextId + 1;
      this.newTodoTitle = "";
    },
    checkAllTodos(event) {
      this.todos.forEach(todo => (todo.completed = event.target.checked));
      this.todos = this.todos;
    },
    updateFilter(newFilter) {
      this.currentFilter = newFilter;
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    handleDeleteTodo(event) {
      this.todos = this.todos.filter(todo => todo.id !== event.id);
    },
    handleToggleComplete(event) {
      const todoIndex = this.todos.findIndex(todo => todo.id === event.id);
      const updatedTodo = {
        ...this.todos[todoIndex],
        completed: !this.todos[todoIndex].completed
      };
      this.todos = [
        ...this.todos.slice(0, todoIndex),
        updatedTodo,
        ...this.todos.slice(todoIndex + 1)
      ];
    }
  }
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 10px auto;
}
.todo-input {
  width: 100%;
  padding: 10px, 20px;
  font-size: 18px;
  margin-bottom: 20px;
}
.inner-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 15px;
  margin-bottom: 13px;
}
.inner-container-input {
  margin-right: 12px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
}
button:hover {
  background: lightseagreen;
}
button:focus {
  outline: none;
}
.active {
  background: lightseagreen;
}
</style>