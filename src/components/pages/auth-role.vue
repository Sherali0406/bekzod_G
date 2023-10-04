<template>
  <AppModal v-model="dialog">
    <h1 class="text-center mb-4">Roleni tanlang:</h1>
    <div class="main" v-for="(item, index) of data" :key="index">
      <button class="btn btn-primary" id="btn" @click="kettu(item)">{{ item }}</button>
    </div>
  </AppModal>
</template>

<script setup>
import AppModal from "../ui/app-modal.vue";
import { ref } from "vue";
import http from "../plugins/axios";
import { createToast } from "mosha-vue-toastify";
const dialog = ref(false);
import { useRouter } from "vue-router";
const router = useRouter();

const toastcreate = () => {
  createToast("Successfully created! (Reloading...)", {
    position: "top-right",
    type: "success",
    transition: "bounce",
  });
};

const openModal = (value) => {
  dialog.value = true;
};

const superadmin = (e) => {
  e.preventDefault();
  router.push({ name: "admin" });
  try {
  } catch (err) {
    console.log(err);
  }
};
defineExpose({ openModal });

const props = defineProps({
  data: {
    type: Array,
  },
});

const kettu = (admin) => {
  if (admin == "admin") router.push({ name: "admin" });
  else if (admin == "superadmin") router.push({ name: "superadmin" });
  try {
  } catch (err) {
    console.log(err);
  }
};
</script>

<style lang="scss" scoped>
.main {
  display: flex;
  justify-content: center;
  gap: 20px;
}

#btn{
  display: flex;
  flex-direction: row;
  background-color: darkred;
  border-color: red;
}
</style>