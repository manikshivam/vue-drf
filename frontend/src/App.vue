<template>
  <div>
    <Login
      v-if="page === 'login' && !isAuth"
      @switch="page = 'register'"
      @login-success="handleLogin"
    />

    <Register
      v-if="page === 'register' && !isAuth"
      @switch="page = 'login'"
      @login-success="handleLogin"
    />

    <Todo v-if="isAuth" />

    <button v-if="isAuth" @click="logout">Logout</button>
  </div>
</template>

<script>
import Login from "./components/Login.vue";
import Register from "./components/Register.vue";
import Todo from "./components/Todo.vue";

export default {
  components: { Login, Register, Todo },

  data() {
    return {
      page: "login",
      isAuth: false
    };
  },

  mounted() {
    const token = localStorage.getItem("token");
    if (token) this.isAuth = true;
  },

  methods: {
    handleLogin() {
      this.isAuth = true;
    },

    logout() {
      localStorage.removeItem("token");
      this.isAuth = false;
      this.page = "login";
    }
  }
};
</script>