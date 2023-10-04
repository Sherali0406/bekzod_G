<template>
  <div class="container">
    <countries ref="countries_modal" />
    <div class="btn-first mb-3" id="btns">
      <button @click="createItem" class="btn btn-danger">Create Product</button>
      <button @click="logOut" class="btn btn-danger">LogOut</button>
    </div>
    <div class="row">
      <div
        class="col-6 my-2"
        v-for="(item, index) in items.products"
        :key="index"
      >
        <div class="card">
          <div class="card-header">
            <h1 class="text-center fs-6">{{ item.email }}</h1>
          </div>
          <div class="card-body">
            <h1 class="fs-3">name:{{ item.name }}</h1>
            <h1 class="fs-3">brand:{{ item.brand }}</h1>
            <h1 class="fs-3">group:{{ item.group }}</h1>
            <h1 class="fs-3">username:{{ item.username }}</h1>
            <h1 class="fs-3">price:{{ item.price }}</h1>
            <h1 class="fs-3">arrival_price:{{ item.arrival_price }}</h1>
            <h1 class="fs-3">selling_price:{{ item.selling_price }}</h1>
            <h1 class="fs-3">description:{{ item.description }}</h1>
            <p>{{ item.username }}</p>
          </div>
          <div class="card-footer">
            <button class="btn btn-success" @click="editItem(item)">
              Edit
            </button>
            <button class="btn btn-success" @click="deleteItem(item._id)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import http from "../../components/plugins/axios";
import countries from "../../components/pages/task.vue";
import { createToast } from "mosha-vue-toastify";

const toastdelete = () => {
  createToast("Deleted", {
    position: "top-right",
    type: "danger",
    transition: "bounce",
  });
};

const items = ref([]);
const countries_modal = ref();

const getCountries = () => {
  http
    .get("products")
    .then((res) => {
      res.data = res.data;
      items.value = res.data;
      console.log(res.data);
    })
    .catch((err) => {
      console.log(err);
    });
};

const deleteItem = (id) => {
  http
    .delete(`/products/${id}`)
    .then((res) => {
      localStorage.setItem(
        "notification",
        JSON.stringify({
          message: "Product is deleted",
          type: "danger",
        })
      );
      location.reload();
    })
    .catch((err) => {
      console.log("error", err);
    });
};

const createItem = () => {
  countries_modal.value.openModal();
};

const editItem = (item) => {
  countries_modal.value.openModal(item);
};

const logOut = () => {
  localStorage.removeItem("token");
  location.reload();
};
getCountries();

</script>
<style lang="scss" scoped>

.button-group {
  display: flex;
  justify-content: space-evenly;
  margin-top: 20px;
}

.card {
  width: 80%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
  margin-bottom: 20px; /* Added a margin to separate cards */
}

#btns{
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.card:hover {
  transform: scale(1.05);
}

.card-header {
  background-color: darkred;
  color: white;
  padding: 10px;
  text-align: center;
}

.card-body {
  padding: 10px; 
  margin-left: 10px;
}

.card-footer {
  background-color: #f1f1f1;
  display: flex;
  justify-content: center;
  gap: 10px;
  padding: 10px;
}

.btn-info,
.btn-danger {
  width: 80px;
  background-color: darkred;
}

/* Additional styles for specific elements if needed */
.container {
  max-width: 1200px;
  margin: 0 auto;
}

</style>