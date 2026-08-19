<script setup>
import { ref } from "vue"

const form = ref({
  name: "",
  email: "",
  password: "",
  passwordConfirm: ""
})
const errors = ref({})
const registeredUser = ref(null)

const validate = () => {
  const found = {}

  if (form.value.name.trim().length < 3) {
    found.name = "Name must be 3 characters or more"
  }

  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.value.email.trim())) {
    found.email = "Enter a valid email address"
  }

  if (form.value.password.length < 8) {
    found.password = "Password must be 8 characters or more"
  }

  if (form.value.passwordConfirm !== form.value.password) {
    found.passwordConfirm = "Passwords do not match"
  }

  errors.value = found
  return Object.keys(found).length === 0
}

const register = () => {
  if (!validate()) {
    return
  }

  registeredUser.value = {
    name: form.value.name.trim(),
    email: form.value.email.trim()
  }

  form.value = { name: "", email: "", password: "", passwordConfirm: "" }
}
</script>

<template>
  <div class="register">
    <h1>Register</h1>

    <p v-if="registeredUser" class="success">
      Welcome, {{ registeredUser.name }}! Your account for {{ registeredUser.email }} is ready.
    </p>

    <form novalidate @submit.prevent="register">
      <label for="name">Name</label>
      <input id="name" v-model="form.name" type="text" autocomplete="name" />
      <p v-if="errors.name" class="errorMessage">{{ errors.name }}</p>

      <label for="email">Email</label>
      <input id="email" v-model="form.email" type="email" autocomplete="email" />
      <p v-if="errors.email" class="errorMessage">{{ errors.email }}</p>

      <label for="password">Password</label>
      <input id="password" v-model="form.password" type="password" autocomplete="new-password" />
      <p v-if="errors.password" class="errorMessage">{{ errors.password }}</p>

      <label for="passwordConfirm">Confirm password</label>
      <input
        id="passwordConfirm"
        v-model="form.passwordConfirm"
        type="password"
        autocomplete="new-password"
      />
      <p v-if="errors.passwordConfirm" class="errorMessage">{{ errors.passwordConfirm }}</p>

      <button type="submit">Create account</button>
    </form>
  </div>
</template>

<style scoped>
.register {
  max-width: 420px;
  margin: 0 auto;
  padding: 16px;
}

h1 {
  font-weight: bold;
  font-size: 3rem;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 1rem;
  margin-bottom: 0.25rem;
}

input {
  padding: 0.5rem;
  border: 1px solid #999;
  border-radius: 0.25rem;
  font-size: 1rem;
}

button {
  margin-top: 1.5rem;
  padding: 0.75rem;
  color: white;
  font-size: 1.25rem;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  background-color: blueviolet;
}

.errorMessage {
  margin-top: 0.25rem;
  color: rgb(176, 19, 19);
}

.success {
  color: rgb(19, 130, 45);
}
</style>
