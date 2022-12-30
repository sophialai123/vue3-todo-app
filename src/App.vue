<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome to Vue Todo App
        <input type="text" placeholder="Name" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO APP</h3>
      <form id="new-todo-form">
        <h4>What is on your todo list?</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="e.g create a new app"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add todo" @click="addTodo" />
      </form>
    </section>
    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div
        class="list"
        id="todo-list"
        v-for="(todo, index) in todos_asc"
        :key="index"
        :class="`todo-item ${todo.done && 'done'}`"
      >
        <label>
          <input type="checkbox" v-model="todo.done" />
          <span :class="`bubble ${todo.category}`"></span>
        </label>
        <div class="todo-content">
          <input type="text" v-model="todo.content" />
        </div>
        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, watch, computed } from "vue";
import "./main.css";
const todos = ref([]);
const name = ref("");
const input_content = ref("");
const input_category = ref(null);

//return ascending list by created date
const todos_asc = computed(() => {
  return [...todos.value].sort((a, b) => (a.createAt = b.createAt));
});

//watch the name value  changes and save in the localstorage
watch(name, (newValue) => localStorage.setItem("name", newValue));

watch(
  todos,
  (newValue) => {
    localStorage.setItem("todos", JSON.stringify(newValue));
  },
  { deep: true }
);

//get the saved value from localstorage when it onmounted
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

const addTodo = () => {
  //return nothing if it empty or null
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createAt: new Date().getTime(),
  });
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t != todo);
};
</script>


