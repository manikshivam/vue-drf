<template>
  <div class="auth-container">
    <h2>Register</h2>

    <input v-model="username" placeholder="Username" />
    <input v-model="email" placeholder="Email" />
    <input v-model="password" type="password" placeholder="Password" />

    <button @click="register">Register</button>

    <p>
      Already have an account?
      <span @click="$emit('switch')">Login</span>
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
      email: "",
      password: ""
    };
  },
  methods: {
    async register() {
      try {
        const res = await axios.post("/api/auth/register/", {
          username: this.username,
          email: this.email,
          password: this.password
        });

        localStorage.setItem("token", res.data.token);

        this.$emit("login-success");
      } catch (err) {
        alert("Error registering");
      }
    }
  }
};
</script>