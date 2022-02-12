
<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
let inputValue = ref<string>("");
type Todo = {
  titile: string;
  isDone: boolean;
};
let todoList = ref<Todo[]>([
  { titile: "速冻鱼吃饭", isDone: false },
  { titile: "速冻鱼睡觉", isDone: false },
  { titile: "速冻鱼学习vue3", isDone: true },
]);
const handlerAdd = () => {
  if (inputValue.value) {
    const todo: Todo = { titile: inputValue.value, isDone: false };
    todoList.value.push(todo);
    inputValue.value = "";
  }
};
const handlerRemove = (index: number) => {
  todoList.value.splice(index, 1);
};
const handlerClear = () => {
  todoList.value = todoList.value.filter((todo) => !todo.isDone);
};
const handlerKeyDown = () => {
  handlerAdd();
};
const active = computed(() => {
  return todoList.value.filter((todo) => todo.isDone === true).length;
});
const all = computed(() => {
  return todoList.value.length;
});
const isAllDone = computed({
  get() {
    return active.value === all.value;
  },
  set(value: boolean) {
    todoList.value.forEach((todo) => (todo.isDone = value));
  },
});
const input = ref(null);
onMounted(() => {
  if (input.value !== null)
    // @ts-ignore
    input.value.focus();
});
</script>

<template>
  <div class="container">
    <input
      ref="input"
      type="text"
      @keyup.enter="handlerKeyDown"
      v-model="inputValue"
    />
    <button @click="handlerAdd">add</button>
    <button @click="handlerClear">clear</button>
    <ul v-if="todoList.length">
      <li
        :class="{ done: todo.isDone }"
        :key="index"
        v-for="(todo, index) in todoList"
      >
        <input type="checkbox" v-model="todo.isDone" />
        {{ todo.titile }}
        <span style="cursor: pointer" @click="handlerRemove(index)">❌</span>
      </li>
      <span>全选</span>
      <input type="checkbox" v-model="isAllDone" />
      <span>
        {{ `${active}/${all}` }}
      </span>
    </ul>
    <div v-else>暂无任务🐳</div>
  </div>
</template>

<style>
.container {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.done {
  text-decoration: line-through;
}
</style>
