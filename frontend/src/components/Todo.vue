<template>
  <div class="container">
    <h2>Todo App</h2>

    <!-- Add Todo -->
    <div class="input-box">
      <input v-model="title" placeholder="Enter task..." />
      <button @click="addTodo">Add</button>
    </div>

    <!-- Todo List -->
    <ul>
      <li v-for="todo in todos" :key="todo.id">

        <!-- EDIT MODE -->
        <div v-if="editId === todo.id" class="edit-box">
          <input v-model="editTitle" />
          <div>
            <button @click="updateTodo(todo)">Save</button>
            <button @click="cancelEdit">Cancel</button>
          </div>
        </div>

        <!-- NORMAL MODE -->
        <div v-else class="todo-item">
          <span :class="{ done: todo.completed }">
            {{ todo.title }}
          </span>

          <div class="actions">
            <button @click="startEdit(todo)">✏️</button>
            <button @click="toggleTodo(todo)">✔</button>
            <button @click="deleteTodo(todo.id)">❌</button>
          </div>
        </div>

      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import "../assets/todo.css";

export default {
  data() {
    return {
      todos: [],
      title: "",
      editId: null,
      editTitle: ""
    };
  },

  mounted() {
    this.getTodos();
  },

  methods: {
    // 🔹 GET TODOS
    async getTodos() {
      try {
        const res = await axios.get("/api/todos/", {
          headers: {
            Authorization: `Token ${localStorage.getItem("token")}`
          }
        });
        this.todos = res.data;
      } catch (err) {
        console.error(err);
      }
    },

    // 🔹 ADD TODO
    async addTodo() {
      if (!this.title) return;

      try {
        await axios.post(
          "/api/create/",
          {
            title: this.title,
            completed: false
          },
          {
            headers: {
              Authorization: `Token ${localStorage.getItem("token")}`
            }
          }
        );

        this.title = "";
        this.getTodos();
      } catch (err) {
        console.error(err);
      }
    },

    // 🔹 DELETE TODO
    async deleteTodo(id) {
      try {
        await axios.delete(`/api/delete/${id}/`, {
          headers: {
            Authorization: `Token ${localStorage.getItem("token")}`
          }
        });

        this.getTodos();
      } catch (err) {
        console.error(err);
      }
    },

    // 🔹 TOGGLE COMPLETE
    async toggleTodo(todo) {
      try {
        await axios.put(
          `/api/update/${todo.id}/`,
          {
            title: todo.title,
            completed: !todo.completed
          },
          {
            headers: {
              Authorization: `Token ${localStorage.getItem("token")}`
            }
          }
        );

        this.getTodos();
      } catch (err) {
        console.error(err);
      }
    },

    // ✏️ START EDIT
    startEdit(todo) {
      this.editId = todo.id;
      this.editTitle = todo.title;
    },

    // ❌ CANCEL EDIT
    cancelEdit() {
      this.editId = null;
      this.editTitle = "";
    },

    // 💾 UPDATE TODO
    async updateTodo(todo) {
      try {
        await axios.put(
          `/api/update/${todo.id}/`,
          {
            title: this.editTitle,
            completed: todo.completed
          },
          {
            headers: {
              Authorization: `Token ${localStorage.getItem("token")}`
            }
          }
        );

        this.editId = null;
        this.editTitle = "";
        this.getTodos();
      } catch (err) {
        console.error(err);
      }
    }
  }
};
</script>