<template>
  <div>
    <div class="adding-todo">
      <h1>Vue Todo App</h1>
      <span>
        <input v-model="newTodo" placeholder="Add new todo" @keyup.enter="addTodo" />
        <button @click="addTodo">Add Todo</button>
      </span>
    </div>

    <div class="todo-holder">
      <div v-for="todo in todos" :key="todo.id" class="each-todo">
        <span>
          <button v-if="!isEditing(todo.id)" @click="editTodo(todo)">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none">
              <path d="M11 2H9C4 2 2 4 2 9v6c0 5 2 7 7 7h6c5 0 7-2 7-7v-2" stroke="#fff" stroke-width="2"
                stroke-linecap="round" stroke-linejoin="round"></path>
              <path
                d="M16.04 3.02 8.16 10.9c-.3.3-.6.89-.66 1.32l-.43 3.01c-.16 1.09.61 1.85 1.7 1.7l3.01-.43c.42-.06 1.01-.36 1.32-.66l7.88-7.88c1.36-1.36 2-2.94 0-4.94-2-2-3.58-1.36-4.94 0Z"
                stroke="#fff" stroke-width="2" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round">
              </path>
              <path d="M14.91 4.15a7.144 7.144 0 0 0 4.94 4.94" stroke="#fff" stroke-width="2" stroke-miterlimit="10"
                stroke-linecap="round" stroke-linejoin="round"></path>
            </svg>
          </button>
          <button v-if="!isEditing(todo.id)" @click="deleteTodo(todo.id)" class="delete">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none">
              <path d="M3.17 7.44 12 12.55l8.77-5.08M12 21.61v-9.07" stroke="#fff" stroke-width="2" stroke-linecap="round"
                stroke-linejoin="round"></path>
              <path
                d="M21.61 9.17v5.66c0 .05 0 .09-.01.14-.7-.61-1.6-.97-2.6-.97-.94 0-1.81.33-2.5.88A3.97 3.97 0 0 0 15 18c0 .75.21 1.46.58 2.06.09.16.2.31.32.45l-1.83 1.01c-1.14.64-3 .64-4.14 0l-5.34-2.96c-1.21-.67-2.2-2.35-2.2-3.73V9.17c0-1.38.99-3.06 2.2-3.73l5.34-2.96c1.14-.64 3-.64 4.14 0l5.34 2.96c1.21.67 2.2 2.35 2.2 3.73Z"
                stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
              <path
                d="M23 18c0 .75-.21 1.46-.58 2.06-.21.36-.48.68-.79.94-.7.63-1.62 1-2.63 1a3.97 3.97 0 0 1-3.42-1.94A3.92 3.92 0 0 1 15 18c0-1.26.58-2.39 1.5-3.12A3.999 3.999 0 0 1 23 18ZM20.07 19.04l-2.12-2.11M20.05 16.96l-2.12 2.11"
                stroke="#fff" stroke-width="2" stroke-miterlimit="10" stroke-linecap="round" stroke-linejoin="round">
              </path>
            </svg>
          </button>
        </span>
        <p v-if="!isEditing(todo.id)">{{ todo.title }}</p>
        <input v-if="isEditing(todo.id)" v-model="editedTodoTitle" @blur="updateTodo(todo)"
          @keyup.enter="updateTodo(todo)" />
      </div>
    </div>
  </div>
</template>

<script>

export default {

  data() {
    return {
      newTodo: "",
      todos: [],
      editingTodoId: null,
      editedTodoTitle: "",
    };
  },

  mounted() {
    this.fetchTodos();
  },

  methods: {
    fetchTodos() {
      this.todos = [
        { id: 1, title: "Learn Vue.js" },
        { id: 2, title: "Build a Vue app" },
      ];
    },

    addTodo() {
      if (this.newTodo.trim() !== "") {
        const newTodo = {
          id: Date.now(),
          title: this.newTodo,
        };
        this.todos.push(newTodo);
        this.newTodo = ""; // Clear the input field
      }
    },

    editTodo(todo) {
      this.editingTodoId = todo.id;
      this.editedTodoTitle = todo.title;
    },

    updateTodo(todo) {
      if (this.editedTodoTitle.trim() !== "") {
        todo.title = this.editedTodoTitle;
        this.editingTodoId = null;
        this.editedTodoTitle = "";
      }
    },

    deleteTodo(id) {
      let ask = confirm('Are you sure you wanna to delete this todo')
      if (ask) {
        const index = this.todos.findIndex((todo) => todo.id === id);
        if (index !== -1) {
          this.todos.splice(index, 1);
        }
      }
    },

    isEditing(todoId) {
      return this.editingTodoId === todoId;
    },
  },
};
</script>




























<!-- <template>
  <div id="app">
    <h1>Vue Todo App</h1>
    <input v-model="newTodo" placeholder="Add new todo" />
    <button @click="addTodo">Add Todo</button>

    <div v-for="todo in todos" :key="todo.id">
      <span v-if="todo.id !== editingTodoId">{{ todo.title }}</span>
      <input v-if="todo.id === editingTodoId" v-model="editedTodoTitle" @blur="updateTodo(todo)"
        @keyup.enter="updateTodo(todo)" />
      <button @click="editTodo(todo)">Edit</button>
      <button @click="deleteTodo(todo.id)">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      editingTodoId: null,
      editedTodoTitle: "",
    };
  },

  mounted() {
    this.fetchTodos();
  },

  methods: {
    fetchTodos() {
      this.todos = [
        { id: 1, title: "Testing Micro-Phone" },
        { id: 2, title: "Testing ,Testing Testing" },
      ];
    },

    addTodo() {
      if (this.newTodo.trim() !== "") {
        const newTodo = {
          id: Date.now(),
          title: this.newTodo,
        };
        this.todos.push(newTodo);
        this.newTodo = "";
      }
    },

    editTodo(todo) {
      this.editingTodoId = todo.id;
      this.editedTodoTitle = todo.title;
    },

    updateTodo(todo) {
      if (this.editedTodoTitle.trim() !== "") {
        todo.title = this.editedTodoTitle;
        this.editingTodoId = null;
        this.editedTodoTitle = "";
      } else {
        alert(" You cannot have an empty todo \n However you can delete the todo");
        this.editedTodoTitle = todo.title;
      }
    },

    deleteTodo(id) {
      let ask = confirm('Are you sure you wanna to delete this todo')
      if (ask) {
        const index = this.todos.findIndex((todo) => todo.id === id);
        if (index !== -1) {
          this.todos.splice(index, 1);
        }
      }
    },
  },
};
</script>

<style>
#app {
  text-align: center;
  margin-top: 2rem;
}

input {
  margin-right: 1rem;
}

div {
  margin-top: 0.5rem;
}
</style> -->
