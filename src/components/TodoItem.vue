<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" :checked="todo.completed" v-on:change="toggleComplete(todo)" />
      <div class="todo-item-label" :class="{completed: todo.completed}">{{todo.title}}</div>
    </div>
    <div class="remove-item" v-on:click="deleteItem(todo)"></div>
  </div>
</template>

<script>
export default {
  name: "todoitem",
  props: ["todo"],
  data() {
    return {
      id: "",
      title: "",
      completed: false
    };
  },
  methods: {
    toggleComplete(todo) {
      this.$emit("toggleComplete", todo);
    },
    deleteItem(todo) {
      this.$emit("deleteItem", todo);
    }
  }
};
</script>

<style>
.todo-item {
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}
.remove-item {
  cursor: pointer;
  margin-left: 15px;
}
.remove-item:hover {
  color: lightseagreen;
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  border: 1px solid white;
  margin-left: 12px;
}
.completed {
  text-decoration: line-through;
  color: grey;
}
</style>