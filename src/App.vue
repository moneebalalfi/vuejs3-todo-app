<template>
  <div class="overlay" v-if="!isUsernameSaved"></div>

  <main>
    <header>
      <span class="greeting">Hello {{ username }}! 😇</span>
      <h1>Vuejs3 Todo Application</h1>
      <div class="modal" v-if="!isUsernameSaved">
        <input
          type="text"
          placeholder="Your Name ?"
          v-model.trim="username"
          @keyup.enter="saveUsername"
        />
      </div>
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
    document.title = "Vuejs Todo App";
    const username = ref(localStorage.getItem("username") || "");
    const isUsernameSaved = ref(localStorage.getItem("isLoggedIn") || false);
    const todoContent = ref("");
    const todos = ref(JSON.parse(localStorage.getItem("todos")) || []);

    function saveUsername() {
      if (username.value === "") return;
      isUsernameSaved.value = true;
      localStorage.setItem("username", username.value);
      localStorage.setItem("isLoggedIn", isUsernameSaved.value);
    }

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
      username,
      saveUsername,
      isUsernameSaved,
    };
  },
};
</script>
