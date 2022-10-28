<template>
  <main>
    <header>
      <h1>Vuejs3 Todo Application</h1>
    </header>

    <CreateTodo todoContent="todoContent" @onAddTodo="addNewTodo" />

    <section class="todo-list" v-if="todos.length > 0">
      <h2 class="title">Todo List</h2>
      <div
        v-for="(todo, i) in todos"
        :key="todo.id"
        class="todo-item"
        :class="{ done: todo.isDone }"
      >
        <h3 class="todo-content">{{ todo.content }}</h3>
        <div class="todo-item-controls">
          <button class="toggle-done-btn" @click="toggleTodoState(todo)">
            Done {{ todo.isDone ? "✓" : "" }}
          </button>
          <button class="remove-btn" @click="removeTodo(i)">Remove</button>
        </div>
      </div>
      <div class="todo-list-controls">
        <p>
          Pending <span>{{ pendingTodos }}</span>
        </p>
        <button
          class="set-all-done-btn"
          v-if="pendingTodos !== 0"
          @click="markAllDone"
        >
          Set All Done
        </button>
      </div>
    </section>
  </main>
</template>

<script>
import { computed, ref } from "vue";
import CreateTodo from "./components/CreateTodo.vue";

export default {
  name: "App",
  components: { CreateTodo },
  setup() {
    const todoContent = ref("");
    const todos = ref([]);

    const pendingTodos = computed(() => {
      return todos.value.filter((todo) => !todo.isDone).length;
    });

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

    const toggleTodoState = (todo) => (todo.isDone = !todo.isDone);
    const removeTodo = (index) => todos.value.splice(index, 1);
    const markAllDone = () =>
      todos.value.forEach((todo) => (todo.isDone = true));

    return {
      todos,
      addNewTodo,
      todoContent,
      toggleTodoState,
      removeTodo,
      markAllDone,

      pendingTodos,
    };
  },
};
</script>
