<template> 
  <section class="sidebar"> 
    <div class="sidebar-left"> 
      <Sidebar :items="sidebar_items" /> 
    </div> 
    <div class="sidebar-right"> 
      <router-view></router-view> 
    </div> 
  </section> 
</template> 
 
<script setup> 
import { ref } from "vue"; 
import Sidebar from "../../components/layouts/Sidebar.vue"; 
const sidebar_items = ref([ 
  { title: "Products", path: "/products" }, 
]); 
</script> 
 
<style lang="scss" scoped> 
.sidebar { 
    width: 100%;; 
  display: flex; 
} 
.sidebar-left { 
  width: 300px; 
  height: 100vh; 
} 
 
.sidebar-right { 
  width: calc(100% - 300px); 
    height: 100vh; 
  overflow: auto; 
 
} 
</style>