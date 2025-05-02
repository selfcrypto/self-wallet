<template>
  <div>
    <settings-header v-bind="$attrs" />

    <div class="settings__block">
      <logo-big class="settings__logo" />
      <div class="settings__buttons">
        <settings-button title="General" @click="$emit('action:general')" />
        <settings-button
          title="Contact support"
          :is-link="true"
          @click="contactSupport"
        />
        <settings-button title="About" @click="$emit('action:about')" />
        <settings-button title="View my recovery phrase" @click="toggleSign" />
      </div>
      <settings-button
        title="Reset wallet"
        :is-red="true"
        @click="$emit('action:reset')"
      />
    </div>

    <!-- <div class="settings__block"> -->
    <!-- <settings-button
        title="Bug bounty program"
        :is-link="true"
        @click="bugAction"
      />
      <settings-button
        title="Privacy and terms"
        :is-link="true"
        @click="privacyAction"
      /> -->
    <!-- </div> -->

    <!-- <div class="settings__block">

    </div> -->

    <div class="settings__copyright">
      <p>Version {{ version }} ({{ buildTime }}) © {{ new Date().getFullYear() }} by MyEtherWallet Inc.</p>
    </div>

    <modal-sign
      v-if="isOpenSign"
      :is-unlock="true"
      v-bind="$attrs"
      @window:close="toggleSign"
      @toggle:forgot="toggleForgot"
    />

    <modal-forgot
      v-if="isForgot"
      :is-forgot="isForgot"
      @toggle:forgot="toggleForgot"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import SettingsHeader from "@action/views/settings/components/settings-header.vue";
import SettingsButton from "@action/views/settings/components/settings-button.vue";
import ModalSign from "@action/views/modal-sign/index.vue";
import ModalForgot from "@action/views/modal-forgot/index.vue";
import LogoBig from "@action/icons/common/logo-icon.vue";
const isOpenSign = ref(false);
const isForgot = ref(false);
const version = process.env.PACKAGE_VERSION;
const buildTime = process.env.BUILD_TIME;
defineEmits<{
  (e: "action:reset"): void;
  (e: "action:support"): void;
  (e: "action:general"): void;
  (e: "action:about"): void;
}>();

const bugAction = () => {
  window.open(
    "https://hackerone.com/myetherwallet?type=team",
    "_blank",
    "noopener"
  );
};

const privacyAction = () => {
  window.open(
    "https://www.myetherwallet.com/privacy-policy",
    "_blank",
    "noopener"
  );
};

const contactSupport = () => {
  window.open("https://selfcrypto.io/", "_blank", "noopener");
};

const toggleSign = () => {
  isOpenSign.value = !isOpenSign.value;
};
const toggleForgot = () => {
  isOpenSign.value = false;
  isForgot.value = !isForgot.value;
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.settings {
  &__logo {
    margin: 0px auto 50px;
    display: block !important;
  }
  &__buttons {
    padding: 12px 10px;
    border: 0.5px solid #CBCBCBB2;
    margin-bottom: 17px;
    border-radius: 4px;
  }
  &__copyright {
    padding: 0px;
    margin-top: 30px;
    text-align: center;
    p {
      font-style: normal;
      font-weight: 400;
      font-size: 12px;
      line-height: 20px;
      color: @tertiaryLabel;
      margin: 0;
    }
  }
}
</style>
