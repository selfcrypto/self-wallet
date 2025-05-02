<template>
  <div class="settings__header">
    <h2>Settings</h2>

    <a class="settings__close" @click="$emit('window:close')">
      <close-icon />
    </a>
  </div>
</template>

<script setup lang="ts">
import CloseIcon from "@action/icons/common/close-icon.vue";

defineEmits<{
  (e: "window:close"): void;
}>();
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.settings {
  &__header {
    width: 100%;
    box-sizing: border-box;
    padding: 0px;
    position: relative;
    z-index: 4;
    margin-bottom: 45px;

    h2 {
      font-style: normal;
      font-weight: 500;
      font-size: 20px;
      line-height: 24px;
      margin: 0;
      color: @primaryLabel;
      text-align: center;
    }
  }
  &__close {
    position: absolute;
    top: -8px;
    right: -8px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0;
    transition: background 300ms ease-in-out;

    &:hover {
      background: @black007;
    }
  }
}
</style>
