<template>
  <div class="background"></div>
  <div class="container">
    <h1>To Do List</h1>
    <div class="form-container">
      <form @submit.prevent="addNewTodo">
        <label class="todoName" for="newTodo">À faire : </label>
        <div class="input-container">
          <input v-model="newTodo" name="newTodo" />
          <button class="btn addTask">Ajouter</button>
        </div>
      </form>
    </div>
    <div class="btn-section">
      <button class="btn removeAll" @click="removeAll">Effacer tout</button>
      <button class="btn markAll" @click="markAllDone">
        Marquer tout comme "Fait"
      </button>
    </div>
    <div class="todo-container" v-for="(todo, index) in todos" :key="todo.id">
      <ul>
        <li class="todo">
          <p :class="{ done: todo.done }" @click="toggleDone(todo)">
            {{ todo.content }}
          </p>

          <button class="btn removeTodo" @click="removeTodo(index)">
            Effacer la tâche
          </button>
        </li>
      </ul>
    </div>
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
      localStorage.setItem("task", [newTodo.value]);
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

<style lang="scss" scoped>
.background {
  background-color: #48c9c1;
  // height: 100vh;
}
.container {
  border: 3px solid #48c9c1;
  border-radius: 20px;
  padding: 40px;
  margin: 20px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
h1 {
  margin-top: -20px;
  text-decoration: underline;
  color: #48c9c1;
}
.form-container {
  display: flex;
  flex-direction: column;
}
.input-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 50px;
}
input {
  border-radius: 20px;
  border: 2px solid #48c9c1;
  text-overflow: ellipsis;
  padding: 5px;
  margin-bottom: 5px;
}
ul {
  list-style-type: none;
}
li {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-left: -40px;
}
p {
  font-size: large;
  font-weight: bold;
}
.btn-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}
.btn {
  border: 1px solid #48c9c1;
  border-radius: 10px;
  color: #fff;
  background-color: #48c9c1;
  font-weight: bold;
  &.removeAll {
    margin: 10px 0;
  }
  &.removeTodo {
    margin: -10px 0;
  }
}
.todo-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
