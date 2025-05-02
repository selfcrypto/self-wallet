<template>
  <div class="new-wallet">
    <div class="new-wallet__top">
        <logo-big class="new-wallet__logo" />
        <h3>Use Names. Not Addresses.</h3>
        <p>SelfCrypto is a wallet that simplifies your crypto experience by replacing complex addresses with easy-to-remember names.</p>
        <p>Send tokens effortlessly by typing the recipient's name instead of their address.</p>
    </div>
    <div class="new-wallet__bottom">
        <div class="new-wallet__buttons">
        <base-button title="Create a new wallet" :click="createAction" />
        <base-button
            title="Restore existing wallet"
            :no-background="true"
            :click="restoreAction"
            :border="true"
        />
        </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import LogoBig from "@action/icons/common/logo-icon.vue";
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
  text-align: center;
  display: flex;
  flex-direction: column;
  height: 100%;
  &__top {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  &__logo {
    margin-bottom: 33px;
  }

  h3 {
    font-style: normal;
    font-weight: 500;
    font-size: 20px;
    line-height: normal;
    letter-spacing: 0px;
    color: @primaryLabel;
    margin: 0 0 28px 0;
  }

  p {
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0px;
    margin: 0 0 16px 0;
    color: @secondaryLabel;
  }

  &__buttons {
    text-align: center;
    a {
      margin-top: 8px;
    }
  }
}
</style>
