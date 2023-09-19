<script setup>
import { ref, computed } from "vue";

let id = 0;

const newTodo = ref("");
const newTodoTime = ref("");
const hideCompleted = ref(false);
const todos = ref([
  { id: id++, text: "時間 : 10:00 事項 : test1", done: true },
  { id: id++, text: "時間 : 11:00 事項 : test2", done: true },
  { id: id++, text: "時間 : 15:00 事項 : test3", done: false },
]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});

function addTodo() {
  todos.value.push({
    id: id++,
    text: `時間 : ${newTodoTime.value} 事項 : ${newTodo.value}`,
    done: false,
  });
  newTodo.value = "";
  newTodoTime.value = "";
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <label>時間 : </label>
    <input v-model="newTodoTime" placeholder="請輸入代辦時間" />
    <br />
    <label>事項 : </label>
    <input v-model="newTodo" placeholder="請輸入代辦事項" />
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
          background-color: black;
          color: white;
          border: none;
          padding: 1px 3px;
          margin-left: 5px;
          cursor: pointer;
        "
        @click="removeTodo(todo)"
      >
        刪除
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
