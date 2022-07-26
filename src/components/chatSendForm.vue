<template>
  <form @submit.prevent="sendMessage" class="vue-chat__form">
    <input
      type="text"
      class="form-control border-0 box-shadow-0"
      placeholder="Type your message"
      required
      v-model="message"
    />
    <button type="submit" class="btn btn-success text-white">Send</button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
export default defineComponent({
  setup(props, { emit }) {
    let message = ref("");

    function sendMessage() {
      emit("send-message", message.value);
      message.value = "";
    }

    return { message, sendMessage };
  },
});
</script>

<style lang="scss">
$btn-width: 80px;

.vue-chat__form {
  width: 100%;
  max-width: 100%;
  margin-bottom: 20px;
  height: 40px;
  display: flex;
  padding: 10px 0;
  border-top: 1px solid rgba(0, 0, 0, 0.34);

  input {
    width: calc(100% - #{$btn-width});
    padding: 20px 0;
    border-radius: 0;
    appearance: none;
    border: none;
    outline:0;
    text-indent: 10px;

    &:focus {
      box-shadow: none;
      outline: none;
    }
  }

  button {
    margin-right: 10px;
    border: none;
    color: #fff;
    padding: 0px;
    font-size: 14px;
    height: auto;
    font-weight: bold;
    text-align: center;
    width: $btn-width;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>