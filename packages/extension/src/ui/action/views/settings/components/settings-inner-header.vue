<template>
  <div class="settings__inner-header">
    <a class="settings__back" @click="$emit('window:back')">
      <arrow-back />
    </a>
    <h2 v-show="isGeneral">General</h2>
    <h2 v-show="isAbout">About</h2>
    <h2 v-show="isSupport">Support</h2>
    <h2 v-show="isPhrase">Recovery phrase</h2>
    <h2 v-show="isReset">Reset wallet?</h2>

    <a class="settings__close" @click="$emit('window:close')">
      <close-icon />
    </a>
  </div>
</template>

<script setup lang="ts">
import CloseIcon from "@action/icons/common/close-icon.vue";
import ArrowBack from "@action/icons/common/arrow-back.vue";
defineEmits<{
  (e: "window:close"): void;
  (e: "window:back"): void;
}>();
defineProps({
  isGeneral: {
    type: Boolean,
    default: false,
  },
  isSupport: {
    type: Boolean,
    default: false,
  },
  isAbout: {
    type: Boolean,
    default: false,
  },
  isPhrase: {
    type: Boolean,
    default: false,
  },
  isReset: {
    type: Boolean,
    default: false,
  },
});
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.settings {
  &__inner-header {
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
  &__back {
    position: absolute;
    top: 0px;
    left: 0px;
    border-radius: 8px;
    cursor: pointer;
    padding: 0px;
    font-size: 0;

    &:hover {
      background: @black007;
    }
  }
}
</style>
