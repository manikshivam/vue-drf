<template>
  <div class="auth-container">
    <h2>Login</h2>

    <input v-model="username" placeholder="Username" />
    <input v-model="password" type="password" placeholder="Password" />

    <button @click="login">Login</button>

    <p>
      Don't have an account?
      <span @click="$emit('switch')">Register</span>
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
      password: ""
    };
  },
  methods: {
    async login() {
      try {
        const res = await axios.post("/api/auth/login/", {
          username: this.username,
          password: this.password
        });

        // save token
        localStorage.setItem("token", res.data.token);

        // emit success
        this.$emit("login-success");
      } catch (err) {
        alert("Invalid credentials");
      }
    }
  }
};
</script>

<style>
.auth-container {
  max-width: 400px;
  margin: auto;
  text-align: center;
}
input {
  display: block;
  margin: 10px auto;
  padding: 8px;
  width: 80%;
}
button {
  padding: 8px 12px;
}
span {
  color: blue;
  cursor: pointer;
}
</style>