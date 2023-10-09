<template>
  <header>
    <h1 class="text-center display-1 text-white py-3">Quotes</h1>
  </header>

  <AddTodo @addTodoEmit="(todo) => $emit('addTodoEmit', todo)" />
  <div class="container">
    <div class="card p-1">
      <div class="mt-5 mb-2 d-flex justify-content-evenly">
        <div class="input-group  mb-2 w-50 ">
          <input
            type="text"
            class="form-control"
            placeholder="Search by name or author"
            v-model="searchQuery"
          />
        </div>
        <div class="dropdown ml-auto">
          <button
            class="btn btn-secondary dropdown-toggle"
            type="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
          >
            Sort
          </button>
          <ul class="dropdown-menu">
            <li
              @click="changeSortType"
              data-sort="newest-to-oldest"
              class="dropdown-item"
            >
              Newest to Oldest
            </li>
            <li
              @click="changeSortType"
              data-sort="oldest-to-newest"
              class="dropdown-item"
            >
              Oldest to Newest
            </li>
            <li
              @click="changeSortType"
              data-sort="alphabetically"
              class="dropdown-item"
            >
              Alphabetically
            </li>
          </ul>
        </div>
      </div>
      <p>
        <span style="text-transform: "
          >Sorted
          <strong>{{ sortType }}</strong
          >.</span
        >
      </p>

      <Todos
        @deleteTodoEmit="(todo: Todo) => $emit('deleteTodoEmit', todo)"
        @updateTodoEmit="(todo: Todo) => $emit('updateTodoEmit', todo)"
        :todos="sortAndFilter(todos)"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Todo } from "@/interfaces";
import { ref, computed } from "vue";
import AddTodo from "../components/AddTodo.vue";
import Todos from "../components/Todos.vue";

const { todos } = defineProps(["todos"]);

// const emit = defineEmits(["addTodoEmit", "deleteTodoEmit"]);
let filterType = ref("all");
let sortType = ref("oldest-to-newest");
let searchQuery = ref("");

function changeFilterType(e: any) {
  filterType.value = e.target.dataset.filter;
}

function changeSortType(e: any) {
  sortType.value = e.target.dataset.sort;
}

function applyFilter(todos: Todo[]) {
  let filteredTodos = [];
  if (filterType.value === "all") {
    filteredTodos = todos;
  } else if (filterType.value === "completed") {
    filteredTodos = todos.filter((todo: Todo) => todo.isDone);
  } else {
    filteredTodos = todos.filter((todo: Todo) => !todo.isDone);
  }
  return filteredTodos;
}

function applySort(todos: Todo[]) {
  if (sortType.value === "oldest-to-newest") {
    todos.sort((t1, t2) => t1.dateAdded - t2.dateAdded);
  } else if (sortType.value === "newest-to-oldest") {
    todos.sort((t1, t2) => t2.dateAdded - t1.dateAdded);
  } else {
    todos.sort((t1, t2) => t1.name.localeCompare(t2.name));
  }
  return todos;
}

function sortAndFilter(todos: Todo[]) {
  let displayedTodos = applyFilter(todos);
  displayedTodos = applySort(displayedTodos);
  if (searchQuery.value) {
    displayedTodos = displayedTodos.filter((todo: Todo) => {
      return (
        todo.text.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        todo.author.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });
  }
  return displayedTodos;
}

const filteredTodos = computed(() => sortAndFilter(todos));
</script>

<style scoped></style>