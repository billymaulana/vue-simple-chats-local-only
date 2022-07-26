<template>
  <form v-if="isVisible" @submit.prevent="authUser" class="vue-chat__welcome p-5">
    <h1 class="my-3 fw-bold">Simple Chat</h1>
    <input
      placeholder="Enter your name"
      type="text"
      required
      class="form-control mt-5"
      v-model="name"
    />
    <button type="submit" class="btn btn-block w-100 btn-primary mt-4">Enter</button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  setup(props, { emit }) {
    let name = ref("");
    let isVisible = ref(true);

    let authUser = () => {
      emit("auth-user", name.value);
      isVisible.value = false;
    };

    return { name, isVisible, authUser };
  },
});
</script>

<style lang="scss">
.vue-chat {
  &__welcome {
    position: absolute;
    z-index: 1;
    text-align: left;
    height: 100%;
    width: 100%;
    background: white;
  }
}
.btn{
  background-color: blue;
  color:white;
}
</style>
