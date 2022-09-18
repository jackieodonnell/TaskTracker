<template>
  <div class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome,
        <input
          type="text"
          class="username"
          placeholder="Name here"
          v-model="name"
        />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form v-on:submit.prevent="addTodo">
        <h4>What's on your to-do list?</h4>
        <input
          type="text"
          placeholder="e.g. pick up dry-cleaning"
          v-model="inputContent"
        />
        <h4>Pick a category:</h4>

        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="Business"
              v-model="inputCategory"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="Personal"
              v-model="inputCategory"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="School"
              v-model="inputCategory"
            />
            <span class="bubble school"></span>
            <div>School</div>
          </label>
        </div>

        <input type="submit" value="Add To-do" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TO-DO LIST:</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
          :key="todo.id"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" v-on:click.prevent="removeTodo(todo)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>


<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const inputContent = ref("");
const inputCategory = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (inputContent.value.trim() === "" || inputCategory.value === null) {
    return;
  }
  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    createdAt: new Date().getTime(),
  });
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>


