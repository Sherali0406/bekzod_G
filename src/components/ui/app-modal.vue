<template>
  <div class="app-modal" :class="{ 'app-modal-show': modelValue }" id="modal">
    <appTransition>
      <div v-if="modelValue" class="app-modal__body">
        <slot></slot>
      </div>
    </appTransition>
  </div>

  <appTransition>
    <span
      @click="$emit('update:modelValue', false)"
      v-if="modelValue"
      class="app-modal-shadow"
    ></span>
  </appTransition>
</template>

<script setup>
import appTransition from "./app-transition.vue";
const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
  width: {
    type: String,
  },
});
</script>

<style lang="scss" scoped>
.app-modal {
  color: #fff;
  position: fixed;
  transform: translate(-50%, -50%);
  left: 50%;
  width: 50%;
  max-height: 600px;
  top: 20%;
  background-color:darkred;
  z-index: 1;
  opacity: 0;
  overflow: auto;
  border-radius: 0.5rem;
  margin-top:150px ;
}

.app-modal-show {
  opacity: 1;
  z-index: 100;
}

::-webkit-scrollbar {
  display: none;
}

.app-modal__body {
  padding: 20px;
}

#modal{
  padding: 21px;
  width: 50%;

  
}

.app-modal-shadow {
  background-color: white;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

@media (max-width: 990px) {
  .app-modal {
    width: 70%;
  }
}

@media (max-width: 768px) {
  .app-modal {
    width: 80%;
  }
}

@media (max-width: 576px) {
  .app-modal {
    width: 90%;
  }
}
</style>
