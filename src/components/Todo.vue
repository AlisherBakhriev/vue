<template>
  <div class="card">
    <div class="card-header "  >
      <div class='bt  d-flex align-items-center'>
        <input
        v-model="todo.author"
        type="text"
        :class="[
          'form-control',
          'border-0',
          'bg-light',
          todo.isDone ? 'text-decoration-line-through' : '',
          'ps-0',
        ]"
        @focusout="updateTodo"
      />
        <button
      id="rm-btn"
     class="btn btn-danger ms-auto p-0 d-flex align-items-center "
     @click="deleteTodo"
       >
     <svg
       xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="16"
      fill="currentColor"
      class="bi bi-x-circle"
      viewBox="0 0 16 16"
     >
      <path
        d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
      />
      <path
        d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"
      />
      </svg>
      </button>


      </div>

     
   
    </div>
    <div class="card-body">
      <h5 class="card-title"> <input
      v-model="todo.genre"
      type="text"
      :class="[
        'form-control',
        'border-0',
        'bg-light',
        todo.isDone ? 'text-decoration-line-through' : '',
        'ps-0',
      ]"
      @focusout="updateTodo"
    /></h5>
      <p class="card-text">
      <div>
        <input
      v-model="todo.text"
      type="text"
      :class="[
        'form-control',
        'border-0',
        'bg-light',
        todo.isDone ? 'text-decoration-line-through' : '',
        'ps-0',
      ]"
      @focusout="updateTodo"
    />

    <div class="small-text">
      <small class="text-secondary">
        Updated {{ new Date(todo.dateAdded).toLocaleDateString() }} @
        {{ new Date(todo.dateAdded).toLocaleTimeString() }}
      </small>
    </div>
      </div>
      </p>
    </div>
  </div>


</template>

<script setup lang="ts">
const { todo } = defineProps(["todo"]);
const emit = defineEmits(["deleteTodoEmit", "updateTodoEmit"]);

function deleteTodo() {
  if (confirm("Вы уверены, что хотите удалить задачу?")) {
    emit("deleteTodoEmit", todo);
  }
}

function updateTodo() {
  todo.dateAdded = Date.now();
  emit("updateTodoEmit", todo);
}
</script>
