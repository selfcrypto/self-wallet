<template>
  <div class="base-input__wrap">
    <input
      v-model="textValue"
      :type="showPassword ? 'text' : type"
      :placeholder="placeholder"
      class="base-input"
      :class="{ error: isError }"
      autofocus
      autocomplete="off"
    />
    <a
      v-if="type == 'password'"
      class="base-input__hide"
      @click="toggleVisibility"
    >
      <visible-icon v-if="showPassword" />
      <hide-icon v-else />
    </a>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import HideIcon from "@action/icons/password/hide-icon.vue";
import VisibleIcon from "@action/icons/password/visible-icon.vue";
const showPassword = ref(false);
const props = defineProps({
  placeholder: {
    type: String,
    default: () => {
      return "";
    },
  },
  type: {
    type: String,
    default: () => {
      return "text";
    },
  },
  value: {
    type: String,
    default: () => {
      return "";
    },
  },
  isError: {
    type: Boolean,
    default: () => {
      return false;
    },
  },
});
const emit = defineEmits(["update:value"]);
const textValue = computed({
  get: () => props.value,
  set: (value) => emit("update:value", value),
});
const toggleVisibility = () => {
  showPassword.value = !showPassword.value;
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";
.base-input {
  outline: none;
  background: transparent;
  border: 1px solid rgba(37, 37, 37, 0.8);
  border-radius: 6px;
  margin: 0;
  padding: 0 40px 0 12px;
  font-style: normal;
  font-weight: normal;
  font-size: 16px;
  line-height: 54px;
  letter-spacing: 0px;
  color: @primaryLabel;
  width: 100%;
  box-sizing: border-box;
  &:focus {
    border: 1px solid @primary;
    line-height: 54px;
  }
  &.error {
    border: 1px solid @error;
    line-height: 54px;
  }
  &__wrap {
    position: relative;
  }
  &__hide {
    position: absolute;
    top: 20px;
    right: 12px;
    cursor: pointer;
    &:active {
      opacity: 0.7;
    }
  }
}
</style>
