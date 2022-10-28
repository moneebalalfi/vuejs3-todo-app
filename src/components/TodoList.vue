<template>
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
</template>

<script>
import { ref, watch, computed } from "vue";

export default {
  props: ["data"],

  setup(props) {
    const todos = ref(props.data);

    const toggleTodoState = (todo) => (todo.isDone = !todo.isDone);
    const removeTodo = (index) => todos.value.splice(index, 1);

    const pendingTodos = computed(() => {
      return todos.value.filter((todo) => !todo.isDone).length;
    });

    const markAllDone = () =>
      todos.value.forEach((todo) => (todo.isDone = true));

    watch(
      todos,
      (newVal) => {
        localStorage.setItem("todos", JSON.stringify(newVal));
      },
      {
        deep: true,
      }
    );

    return {
      todos,
      toggleTodoState,
      removeTodo,
      pendingTodos,
      markAllDone,
    };
  },
};
</script>
