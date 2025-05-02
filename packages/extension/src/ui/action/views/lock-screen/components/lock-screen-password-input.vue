<template>
  <div class="lock-screen-password-input">
    <base-input
      type="password"
      placeholder="Password"
      class="lock-screen-password-input__input"
      :is-error="isError"
      v-bind="$attrs"
    />
    <p v-show="isError" class="lock-screen-password-input__error">
      Wrong password
    </p>
  </div>
</template>

<script setup lang="ts">
import BaseInput from "@action/components/base-input/index.vue";
defineProps({
  isError: {
    type: Boolean,
    default: () => {
      return false;
    },
  },
});
</script>

<style lang="less">
@import "~@action/styles/theme.less";
.lock-screen-password-input {
  padding: 0 0 14px 0;
  width: 100%;
  position: relative;
  box-sizing: border-box;
  &__input {
    margin: 0;
  }
  &__error {
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0px;
    color: @error;
    margin: 5px 0px 0px;
    left: 12px;
    bottom: 15px;
  }
}
</style>
