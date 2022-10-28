<template>
  <main>
    <header>
      <h1>Vuejs3 Todo Application</h1>
    </header>

    <CreateTodo :todoContent="todoContent" @onAddTodo="addNewTodo" />

    <TodoList :data="todos" />
  </main>
</template>

<script>
import { ref } from "vue";
import CreateTodo from "./components/CreateTodo.vue";
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: { CreateTodo, TodoList },
  setup() {
    const todoContent = ref("");
    const todos = ref([]);

    function addNewTodo(data) {
      const { textareaRef, todoContent } = data;

      if (!todoContent.value || !textareaRef) {
        textareaRef.value.focus();
        return;
      }

      todos.value.push({
        id: 0,
        content: todoContent.value,
        isDone: !true,
      });
      todoContent.value = "";
    }

    return {
      todos,
      addNewTodo,
      todoContent,
    };
  },
};
</script>
