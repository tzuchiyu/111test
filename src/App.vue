<script setup lang="ts">
import { computed } from "vue";
import { useAppStore } from "@/store/modules/app";
import { ConfigGlobal } from "@/components/ConfigGlobal";
import { isDark } from "@/utils/is";
import { useDesign } from "@/hooks/web/useDesign";
import { useStorage } from "@/hooks/web/useStorage";

const { getPrefixCls } = useDesign();

const prefixCls = getPrefixCls("app");

const appStore = useAppStore();

const currentSize = computed(() => appStore.getCurrentSize);

const greyMode = computed(() => appStore.getGreyMode);

const { getStorage } = useStorage();

const setDefaultTheme = () => {
  if (getStorage("isDark") !== null) {
    appStore.setIsDark(getStorage("isDark"));
    return;
  }
  const isDarkTheme = isDark();
  appStore.setIsDark(isDarkTheme);
};

setDefaultTheme();
</script>

<template>
<<<<<<< HEAD
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
=======
  <ConfigGlobal :size="currentSize">
    <RouterView :class="greyMode ? `${prefixCls}-grey-mode` : ''" />
  </ConfigGlobal>
>>>>>>> b6b94c19a1c892a29cce3d76e1422710cdf5269c
</template>

<style lang="less">
@prefix-cls: ~"@{namespace}-app";

.size {
  width: 100%;
  height: 100%;
}

html,
body {
  padding: 0 !important;
  margin: 0;
  // overflow: hidden;
  .size;

  #app {
    .size;
  }
}

.@{prefix-cls}-grey-mode {
  filter: grayscale(100%);
}
</style>
