<template>
  <div class="card container mt-5" style="background-color:darkred;">
    <authRole :data="modalData.rollar" ref="auth_modal" />
    <div class="card-header text-center">
      <h1 style="color: white;">Login</h1>
    </div>
    <div class="card-body text-center">
      <Form class="d-flex text-center" @submit="savebnt">
        <Field
          rules="required"
          :modelValue="info.username"
          v-slot="{ errors }"
          name="Username"
        >
          <input
            type="text"
            placeholder="Username"
            class="form-control"
            v-model="info.username"
          />
          <p class="text-danger" v-if="errors && errors.length">
            {{ errors[0] }}
          </p>
        </Field>

        <Field
          rules="required"
          :modelValue="info.password"
          v-slot="{ errors }"
          name="Password"
        >
          <input
            type="password"
            placeholder="Password"
            class="form-control"
            v-model="info.password"
          />
          <p class="text-danger" v-if="errors && errors.length">
            {{ errors[0] }}
          </p>
        </Field>

        <div class="card-footer">
          <button type="submit" class="btn btn-success">Sign In</button>
        </div>
      </Form>
    </div>
  </div>
  <template>
    <button @click="toast">Toast it!</button>
  </template>
</template>

<script setup>
import { ref } from "vue";
import $http from "../components/plugins/axios";
import { Form, Field } from "vee-validate";

const info = ref({
  username: "",
  password: "",
});

let roles = ref();
const savebnt = () => {
  $http
    .post("/admins/login", {
      username: info.value.username,
      password: info.value.password,
    })
    .then((res) => {
      roles = res.data.roles;
      auth_modal.value.openModal();
      modalData.value.rollar = roles;
      if (res.data.token) {
        localStorage.setItem("token", res.data.token);
      }
    })
    .catch((err) => {
      toast();
      console.log(err);
    });
};

import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";
const toast = () => {
  createToast("Name or password is wrong!", {
    position: "top-right",
    type: "danger",
    transition: "bounce",
  });
};

import authRole from "../components/pages/auth-role.vue";
const auth_modal = ref();

const modalData = ref({
  rollar: roles,
});
</script>

<style lang="scss" scoped>
.d-flex {
justify-content: center;
align-items: center;
flex-direction: column;
gap: 20px;
box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}
.form-control {
width: 50%;
}
.card {
box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}
</style>