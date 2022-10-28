<template>
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
</template>

<script>
import { ref } from "vue";

export default {
  name: "CreateTodo",
  setup(props, ctx) {
    const todoContent = ref(props.todoContent);
    const textareaRef = ref(null);

    function addNewTodo() {
      const data = {
        todoContent,
        textareaRef,
      };
      ctx.emit("onAddTodo", data);
    }

    return {
      todoContent,
      addNewTodo,
      textareaRef,
    };
  },
};
</script>
