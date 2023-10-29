<template>
  <div class="page auth-page login-page">
    <div class="form-wrapper">
      <h3 class="text-center mb-3">Login</h3>

      <form @submit.prevent="submitLoginForm">
        <div class="form-group">
          <input
            type="text"
            v-model="loginForm.username"
            class="form-control"
            placeholder="username"
          />
        </div>
        <!-- form-group -->

        <div class="form-group">
          <input
            type="password"
            v-model="loginForm.password"
            class="form-control"
            placeholder="password"
          />
        </div>
        <!-- form-group -->

        <button class="btn btn-primary" type="submit">login</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const loginForm = {
  username: "",
  password: "",
};

const submitLoginForm = async (username, password) => {
  username = loginForm.username;
  password = loginForm.password;

  const { data, pending, error, refresh } = await useFetch(
    `https://dummyjson.com/auth/login`,
    {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        username: username, // username
        password: password, // password
      }),

      onResponse({ request, response, options }) {
        if (response.status === 200) {
          localStorage.setItem("token", response._data.token);
          router.push("/users");
        }
      },

      onResponseError({ request, response, options }) {
        console.log(response);
        alert(response._data.message);
      },
    }
  );
};
</script>

<style lang="scss" scoped>
.login-page {
  font-size: 16px;
}
.form-wrapper {
  max-width: 600px;
  margin: auto;
  background: #fff;
  padding: 30px;
  border-radius: 5px;
}

.auth-page {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: url(../../public/assets/login-bg.jpg) no-repeat center center fixed;
  background-size: cover;
}
</style>
