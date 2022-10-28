<template>
  <main>
    <header>
      <h1>Vuejs3 Todo Application</h1>
    </header>

    <section class="create-todo">
      <form @submit.prevent="addNewTodo">
        <div class="form-control">
          <label for="new-todo">New Todo</label>
          <textarea
            id="new-todo"
            placeholder="Exploring Vuejs .."
            rows="3"
            v-model.trim="todoContent"
            ref="textareaRef"
          />
        </div>
        <button class="add-btn">Add</button>
      </form>
    </section>

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
        <!-- <h4>Pending 4</h4>
        <button @click="markAllDone">All done</button> -->
        <!-- To be refactored -->
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

    const textareaRef = ref(null);

    const addNewTodo = () => {
      if (todoContent.value === "") {
        textareaRef.value.focus();
        return;
      }

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
      textareaRef,
    };
  },
};
</script>
