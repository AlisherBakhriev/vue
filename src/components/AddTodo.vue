<template>
  <div>
    <div
      class="container text-center mb-5 bg- w-50 p-5"
      style="height: 500px; background: #cacaca; border-radius: 10px"
    >
      <div class="form-floating" style="margin-top: 50px">
        <input
          id="add-todo"
          class="form-control"
          type="text"
          placeholder="Enter todo"
          v-model="newTodo.text"
          @keyup.enter="addTodo"
        />
        <label for="add-todo" class="form-label">Add Todo</label>
      </div>
      <div class="form-floating mt-3">
        <input
          id="add-author"
          class="form-control"
          type="text"
          placeholder="Enter author"
          v-model="newTodo.author"
          @keyup.enter="addTodo"
        />
        <label for="add-author" class="form-label">Add Author</label>
      </div>

      <div class="form-floating mt-3">
        <input
          id="add-genre"
          class="form-control"
          type="text"
          placeholder="Enter genre(s)"
          v-model="newTodo.genre"
          @keyup.enter="addTodo"
        />
        <label for="add-genre" class="form-label">Add Genre(s)</label>
      </div>

      <div class="d-grid gap-2 col-6 mx-auto mt-4">
        <button
          class="btn text-white"
          style="background: #f8501a"
          @click="addTodo"
        >
          Add Todo
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Todo } from "@/interfaces";
import { ref } from "vue";
const newTodo = ref<Todo>({
  text: "",
  author: "",
  genre: "",
  dateAdded: 0,
});
const emit = defineEmits(["addTodoEmit"]);

function addTodo() {
  if (newTodo.value.text && newTodo.value.author) {
    newTodo.value.dateAdded = Date.now();
    emit("addTodoEmit", newTodo.value);
    newTodo.value = {
      text: "",
      author: "",
      genre: "",
      dateAdded: 0,
    };
  }
}
</script>

<style scoped>
.form-label {
  width: 250px;
}
</style>
