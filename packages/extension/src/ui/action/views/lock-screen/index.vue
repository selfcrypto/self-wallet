<template>
  <div class="lock-screen__container">
    <div
      v-show="!isForgot && !isLocked && !isUnlocking"
      class="lock-screen__wrap"
    >
      <div class="lock-screen__top">
        <logo-big class="lock-screen__logo" />
        <p>The revolutionary wallet for Ethereum and all EVM-compatible chains</p>
      </div>  
      <div class="lock-screen__bottom">
        <h4>Unlock with password</h4>
        <lock-screen-password-input
            :is-error="isError"
            :value="password"
            @update:value="passwordChanged"
            @keyup.enter="unlockAction"
        />
        <base-button
            title="Unlock"
            :click="unlockAction"
            :disabled="isDisabled"
        />
        <base-button
            title="Import wallet"
            :no-background="true"
        />
      </div>
    </div>

    <div v-show="isUnlocking" class="lock-screen__unlocking">
      <swap-looking-animation />
    </div>

    <lock-screen-forgot
      v-show="isForgot"
      :reset="resetAction"
      @toggle:forgot="toggleForgot"
    />

    <lock-screen-timer v-show="isLocked" :close="closeLockedAction" />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import LogoBig from "@action/icons/common/logo-big.vue";
import BaseButton from "@action/components/base-button/index.vue";
import LockScreenPasswordInput from "./components/lock-screen-password-input.vue";
import LockScreenForgot from "./components/lock-screen-forgot.vue";
import LockScreenTimer from "./components/lock-screen-timer.vue";
import { sendToBackgroundFromAction } from "@/libs/messenger/extension";
import { InternalMethods } from "@/types/messenger";
import { computed } from "@vue/reactivity";
import SwapLookingAnimation from "@action/icons/swap/swap-looking-animation.vue";
import KeyRing from "@/libs/keyring/keyring";
import { initAccounts } from "@/libs/utils/initialize-wallet";
import { trackGenericEvents } from "@/libs/metrics";
import { GenericEvents } from "@/libs/metrics/types";

const emit = defineEmits<{
  (e: "update:init"): void;
}>();

const password = ref(process.env.PREFILL_PASSWORD!);
const isDisabled = computed(() => {
  return password.value.length < 5 || isUnlocking.value;
});
const isError = ref(false);
const isForgot = ref(false);
const isLocked = ref(false);
const isUnlocking = ref(false);

const unlockAction = async () => {
  isUnlocking.value = true;
  const unlockStatus = await sendToBackgroundFromAction({
    message: JSON.stringify({
      method: InternalMethods.unlock,
      params: [password.value.trim()],
    }),
  });
  if (unlockStatus.error) {
    isError.value = true;
    isUnlocking.value = false;
    trackGenericEvents(GenericEvents.login_error);
  } else {
    isError.value = false;
    const privateKeyring = new KeyRing();
    await privateKeyring.unlock(password.value.trim());
    await initAccounts(privateKeyring);
    password.value = "";
    emit("update:init");
    setTimeout(() => (isUnlocking.value = false), 750);
    trackGenericEvents(GenericEvents.login_success);
  }
};
const forgotAction = () => {
  toggleForgot();
};
const passwordChanged = (text: string) => {
  password.value = text;
  isError.value = false;
};
const toggleForgot = () => {
  isForgot.value = !isForgot.value;
};
const resetAction = () => {
  password.value = "";
};
const closeLockedAction = () => {
  isLocked.value = false;
};
</script>

<style lang="less" scoped>
@import "~@action/styles/theme.less";
.lock-screen {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  &__container {
    width: 100%;
    height: 600px;
    //   width: 454px;
    //   height: 397px;
    overflow-x: hidden;
    overflow-y: hidden;
    position: fixed;
    left: 0;
    top: 0;
    z-index: 999;
    background: radial-gradient(
        100% 50% at 100% 50%,
        rgba(250, 250, 250, 0.92) 0%,
        rgba(250, 250, 250, 0.98) 100%
      )
      @primary;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }
  &__wrap {
    box-sizing: border-box;
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    text-align: center;
    h4 {
        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: normal;
        letter-spacing: 0px;
        color: @primaryLabel;
        margin: 0 0 8px 0;
    }
  }
  &__top {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    background: @primaryLabel;
    p {
        color: @gray03;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 24px;
        margin: 0;
        max-width: 260px;
    }
  }
  &__bottom {
    padding: 20px 20px 0px;
  }
  &__unlocking {
    width: 300px;
    height: 300px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    position: relative;

    svg {
      width: 132px;
      position: relative;
      z-index: 2;
    }
  }
  &__logo {
    margin-bottom: 24px;
  }
  &__forgot {
    position: absolute;
    width: 320px;
    left: 50%;
    margin-left: -160px;
    bottom: 20px;
  }
}
</style>
