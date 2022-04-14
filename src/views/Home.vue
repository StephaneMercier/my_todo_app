<template>
  <h1>To Do App</h1>
  <form @submit.prevent="addNewTodo">
    <label class="todoName" for="newTodo">Nouvelle Tâche</label>
    <input v-model="newTodo" name="newTodo" />
    <button>Ajouter</button>
  </form>
  <button @click="removeAll">Effacer tout</button>
  <button @click="markAllDone">Marquer tous comme "Fait"</button>
  <div v-for="(todo, index) in todos" :key="todo.id">
    <ul>
      <li class="todo">
        <h3 :class="{ done: todo.done }" @click="toggleDone(todo)">
          {{ todo.content }}
        </h3>
        <button @click="removeTodo(index)">Effacer la tâche</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);
    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
      });
      newTodo.value = "";
    }
    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
    }

    function markAllDone() {
      todos.value.forEach((todo) => (todo.done = true));
    }

    function removeAll() {
      todos.value = [];
    }

    return {
      todos,
      newTodo,
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAll,
    };
  },
};
</script>

<style scoped>
h1 {
  text-decoration: underline;
}
ul {
  list-style-type: none;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
