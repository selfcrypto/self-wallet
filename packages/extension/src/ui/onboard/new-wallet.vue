<template>
  <div class="new-wallet">
    <logo-big class="new-wallet__logo" />
    <h3>Use Names.<br />Not Addresses.</h3>
    <p>
      SelfCrypto is a wallet that simplifies your crypto experience by replacing
      complex addresses with easy-to-remember names.
    </p>
    <p>
      Send tokens effortlessly by typing the recipient's name instead of their
      address.
    </p>

    <div class="new-wallet__buttons">
      <base-button title="Create a new wallet" :click="createAction" />
      <base-button
        title="Restore existing wallet"
        :no-background="true"
        :click="restoreAction"
      />
    </div>
  </div>
</template>
<script setup lang="ts">
import LogoBig from "@action/icons/common/logo-big.vue";
import BaseButton from "@action/components/base-button/index.vue";
import { routes as cwalletRoutes } from "./create-wallet/routes";
import { routes as rwalletRoutes } from "./restore-wallet/routes";
import { useRouter } from "vue-router";

const router = useRouter();

const createAction = () => {
  router.push({ name: cwalletRoutes.pickPassword.name });
};

const restoreAction = () => {
  router.push({ name: rwalletRoutes.start.name });
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.new-wallet {
  &__logo {
    margin-bottom: 24px;
  }

  h3 {
    font-style: normal;
    font-weight: 700;
    font-size: 34px;
    line-height: 40px;
    letter-spacing: 0.25px;
    color: @primaryLabel;
    margin: 0 0 16px 0;
  }

  p {
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0.25px;
    margin: 0 0 16px 0;
    color: @primaryLabel;
  }

  &__buttons {
    text-align: center;

    a {
      margin-top: 8px;
    }
  }
}
</style>
