<template>
  <main>
    <header>
      <h1>Vuejs3 Todo Application</h1>
    </header>
    <section class="create-todo">
      <form @submit.prevent="addNewTodo">
        <label for="new-todo">New Todo</label>
        <input id="new-todo" v-model="todoContent" />
        <button>Add</button>
      </form>
      <button @click="markAllDone">Mark all done</button>
    </section>
    <section class="todo-list">
      <div
        v-for="(todo, i) in todos"
        :key="todo.id"
        @click="toggleTodoState(todo)"
        :class="{ done: todo.isDone }"
      >
        <h3>{{ todo.content }}</h3>
        <button @click="removeTodo(i)">Remove</button>
      </div>
    </section>
  </main>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  components: {},
  setup() {
    const todoContent = ref("");
    const todos = ref([]);

    const addNewTodo = () => {
      todos.value.push({
        id: 0,
        content: todoContent.value,
        isDone: !true,
      });
      todoContent.value = "";
    };

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
    };
  },
};
</script>
