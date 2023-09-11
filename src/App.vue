<script setup>
import { ref, computed } from "vue";

let id = 0;

const newTodo = ref("");
const hideCompleted = ref(false);
const todos = ref([
  { id: id++, text: "test1", done: true },
  { id: id++, text: "test2", done: true },
  { id: id++, text: "test3", done: false },
  { id: id++, text: "test4", done: false },
  { id: id++, text: "test5", done: false },
]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false });
  newTodo.value = "";
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" />
    <button
      style="
        background-color: coral;
        color: white;
        border: none;
        padding: 2px 5px;
        margin-left: 5px;
      "
    >
      新增
    </button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button
        style="
          background-color: blueviolet;
          color: white;
          border: none;
          padding: 1px 3px;
          margin-left: 5px;
          cursor: pointer;
        "
        @click="removeTodo(todo)"
      >
        X
      </button>
    </li>
  </ul>
  <button
    style="
      background-color: coral;
      color: white;
      border: none;
      padding: 5px 2px;
    "
    @click="hideCompleted = !hideCompleted"
  >
    {{ hideCompleted ? "顯示全部" : "隱藏已完成" }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
