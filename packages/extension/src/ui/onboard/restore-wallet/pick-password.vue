<template>
  <div class="pick-password">
    <logo-big class="new-wallet__logo" />
    <h3 class="pick-password__title">Pick a password</h3>
    <p class="pick-password__description">
      This will be used to unlock your wallet.
    </p>

    <div class="pick-password__form">
      <password-input
        @update:strength-and-password="passwordUpdated"
        @keyup.enter="nextAction()"
      />
      <base-button title="Next" :click="nextAction" :disabled="isDisabled" />
    </div>

    <p class="pick-password__label">
      Best passwords are long and contain letters, numbers and special characters.
    </p>
  </div>
</template>
<script setup lang="ts">
import { ref, onMounted } from "vue";
import LogoBig from "@action/icons/common/logo-icon.vue";
import BaseButton from "@action/components/base-button/index.vue";
import PasswordInput from "@action/components/password-input/index.vue";
import { useRouter } from "vue-router";
import { routes } from "./routes";
import { useRestoreStore } from "./store";
const store = useRestoreStore();
const router = useRouter();

const password = ref("");
const isDisabled = ref(true);

const nextAction = () => {
  if (!isDisabled.value) {
    store.setPassword(password.value);
    router.push({
      name: routes.typePassword.name,
    });
  }
};

const passwordUpdated = (info: { password: string; strength: number }) => {
  password.value = info.password.trim();
  isDisabled.value = true;
  if (info.strength > 1) isDisabled.value = false;
};

onMounted(() => {
  if (!store.mnemonic) {
    router.push({ name: routes.start.name });
  }
});
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.pick-password {
  width: 100%;
  text-align: center;
  &__title {
    font-style: normal;
    font-weight: 500;
    font-size: 20px;
    line-height: normal;
    letter-spacing: 0px;
    color: @primaryLabel;
    margin: 0 0 21px 0;
  }

  &__description {
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 24px;
    color: @secondaryLabel;
    margin: 0;
  }

  &__form {
    padding: 21px 0 12px;
  }

  &__label {
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    color: @primaryLabel;
    margin: 0;
  }
}
</style>
