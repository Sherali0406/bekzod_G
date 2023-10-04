<template>
  <AppModal v-model="dialog">
    <h1 v-if="!forms._id">Add Admin</h1>
    <h1 v-else>Edit Admin</h1>
    <form>
      <input
        type="text"
        v-model="forms.name"
        class="form-control my-2"
        placeholder="Name"
      />
      <input
        type="text"
        v-model="forms.brand"
        class="form-control my-2"
        placeholder="Brand"
      />
      <input
        type="text"
        v-model="forms.group"
        class="form-control my-2"
        placeholder="Group"
      />
      <input
        type="username"
        v-model="forms.username"
        class="form-control my-2"
        placeholder="Username"
      />
      <input
        type="number"
        v-model="forms.price"
        class="form-control my-2"
        placeholder="Price"
      />
      <input
        type="number"
        v-model="forms.arrival_price"
        class="form-control my-2"
        placeholder="Arrival_price"
      />
      <input
        type="number"
        v-model="forms.selling_price"
        class="form-control my-2"
        placeholder="Selling_price"
      />
      <input
        type="text"
        v-model="forms.description"
        class="form-control my-2"
        placeholder="Description"
      />
      <input
        type="password"
        v-model="forms.password"
        class="form-control my-2"
        placeholder="Password"
      />
      <input
        type="password"
        v-model="forms.confirm_password"
        class="form-control my-2"
        placeholder="Confirm password"
      />
      <button class="btn btn-danger" @click="save">save</button>
    </form>
  </AppModal>
</template>

<script setup>
import AppModal from "../ui/app-modal.vue";
import { ref, watch } from "vue";
import http from "../plugins/axios";
import { createToast } from "mosha-vue-toastify";
import { Form, Field } from "vee-validate";
const dialog = ref(false);

const toastcreate = () => {
  createToast(" Created! ", {
    position: "top-right",
    type: "success",
    transition: "bounce",
  });
};

const toastedit = () => {
  createToast(" Edited ", {
    position: "top-right",
    type: "success",
    transition: "bounce",
  });
};

const forms = ref({
  name: "",
  brand: "",
  group: "",
  username: "",
  price:null,
  arrival_price:null,
  selling_price:null,
  description:"",
  password: "",
  confirm_password: "",
});

watch(dialog, (value) => {
  if (!value) {
    forms.value = {};
  }
});

const openModal = (value) => {
  if (value) forms.value = { ...value };
  dialog.value = true;
};

const save = (e) => {
  e.preventDefault();
  try {
    if (!forms.value._id) {
      http
        .post("/products/add", {
          name: forms.value.name,
          brand: forms.value.brand,
          group: forms.value.group,
          username: forms.value.username,
          price: forms.value.price,
          arrival_price: forms.value.arrival_price,
          selling_price: forms.value.selling_price,
          description: forms.value.description,
          password: forms.value.password,
          confirm_password: forms.value.confirm_password,
        })
        .then((res) => {
          console.log(res);
          toastcreate();
          setTimeout(() => {
            location.reload();
          }, 3000);
        });
    } else {
      http
        .patch(`/products/update/${forms.value._id}`, {
          name: forms.value.name,
          brand: forms.value.brand,
          group: forms.value.group,
          username: forms.value.username,
          price: forms.value.price,
          arrival_price: forms.value.arrival_price,
          selling_price: forms.value.selling_price,
          description: forms.value.description,
          password: forms.value.password,
        })
        .then((res) => {
          console.log(res);
          toastedit();
          setTimeout(() => {
            location.reload();
          }, 3000);
        });
    }
  } catch (err) {
    console.log(err);
  }
};
defineExpose({ openModal });
</script>

<style lang="scss" scoped></style>