<template>
  <div class="vue-chat__header d-flex align-items-center justify-content-start bg-primary p-2">
    <img
      src="https://www.whatspaper.com/wp-content/uploads/2021/10/hd-star-wars-wallpaper-whatspaper.png"
      alt=""
      class="vue-chat__header-img"
    />
    <div class="vue-chat__header-name text-white text-left fw-bold text-capitalize">
      <slot></slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  setup() {},
});
</script>

<style lang="scss">
.vue-chat {
  &__header {
    height: 50px;

    &-img {
      width: 30px;
      height: 30px;
      border-radius: 50%;
    }

    &-name {
      margin-left: 10px;
    }
  }
}
</style>