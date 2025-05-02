<template>
  <div class="recovery-phrase">
    <div class="recovery-phrase__top">
        <h3>Secret recovery phrase</h3>
        <div class="recovery-phrase__list">
            <p>This is the recovery phase for your wallet. You and you alone have access to it. It can be used to restore your wallet.</p>
            <p>Best practices for your recovery phrase are to write it down on paper and store it somewhere secure. Resist temptation to email it to yourself or screenshot it.</p>
        </div>
        <div class="recovery-phrase__wrap">
            <div
            v-for="(phrase, index) in set"
            :key="index"
            class="recovery-phrase__item"
            >
            <span>{{ index + 1 }}.</span> {{ phrase }}
            </div>
        </div>
        <div class="recovery-phrase__copy">
            <p @click="copyPhrase"><copyIcon />Copy pharse</p>
        </div>
    </div>
    <div class="recovery-phrase__bottom">
        <base-button title="Next" :click="nextAction" />
    </div>
  </div>
</template>
<script setup lang="ts">
import BaseButton from "@action/components/base-button/index.vue";
import { useRouter } from "vue-router";
import { onMounted, computed, ref } from "vue";
import { generateMnemonic } from "bip39";
import { routes } from "./routes";
import { useOnboardStore } from "./store";
import copyIcon from "@action/icons/actions/copy.vue";

const router = useRouter();
const store = useOnboardStore();

const password = store.password;
const mnemonic = ref("");

const nextAction = () => {
  store.setMnemonic(mnemonic.value);
  router.push({
    name: routes.checkPhrase.name,
  });
};

onMounted(() => {
  createMnemonic();
  checkForPassword();
});

const checkForPassword = () => {
  if (!password) {
    router.push({ path: routes.pickPassword.path });
  }
};

const createMnemonic = () => {
  mnemonic.value = generateMnemonic(128);
};

const set = computed(() => {
  const copy = mnemonic.value.split(" ");
  return copy;
});
const firstSet = computed(() => {
  const copy = mnemonic.value.split(" ");
  return copy.splice(0, 6);
});
const secondSet = computed(() => {
  const copy = mnemonic.value.split(" ");
  return copy.splice(6);
});

const copyPhrase = async () => {
    try {
        const pharseArr = mnemonic.value.split(" ");
        const formattedText = pharseArr.map((word, index) => `${index + 1}: ${word}`).join("\n");
        await navigator.clipboard.writeText(formattedText);
        console.log("Copied successfully!");
    } catch (err) {
        console.error("Clipboard copy failed:", err);
    }
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.recovery-phrase {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  &__top {
    margin-top: 20px;
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  h3 {
    font-style: normal;
    font-weight: 500;
    font-size: 20px;
    line-height: normal;
    letter-spacing: 0px;
    color: @primaryLabel;
    margin: 0;
    position: absolute;
    top: 16px;
    left: 0px;
    right: 0px;
    width: 100%;
    text-align: center;
  }


  &__list {
    margin-bottom: 20px;
    p {
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 20px;
        color: @secondaryLabel;
        margin: 0 0 10px 0;
        padding: 8px 20px;
        border-radius: 6px;
        position: relative;
        overflow: hidden;
        border: 1px solid rgba(37, 37, 37, 0.2);
        &::before {
            content: "";
            background-color: #EB4242;
            width: 8px;
            height: 100%;
            position: absolute;
            top: 0px;
            left: 0px;
        }
    }
  }

  &__wrap {
    border: 1px solid rgba(37, 37, 37, 0.2);
    box-sizing: border-box;
    border-radius: 6px;
    margin-bottom: 20px;
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    justify-content: space-around;
    flex-wrap: wrap;
  }

  &__block {
    width: 50%;
  }

  &__item {
    position: relative;
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 40px;
    letter-spacing: 0px;
    color: @primaryLabel;
    -webkit-touch-callout: text;
    -webkit-user-select: text;
    -khtml-user-select: text;
    -moz-user-select: text;
    -ms-user-select: text;
    user-select: text;
    flex: 33%;
    box-sizing: border-box;
    border-right: 1px solid rgba(37, 37, 37, 0.2);
    border-bottom: 1px solid rgba(37, 37, 37, 0.2);
    text-align: center;
    &:nth-child(3n) {
        border-right: none;
    }
    &:nth-last-child(-n+3) {
        border-bottom: none;
    }
    &:nth-child(2),
    &:nth-child(5),
    &:nth-child(8),
    &:nth-child(11) {
        background: @white;
    }

    span {
      font-style: normal;
      font-weight: 400;
      font-size: 11px;
      line-height: normal;
      letter-spacing: 0px;
      color: @secondaryLabel;
      position: absolute;
      left: 8px;
      top: 5px;
    }
  }

  &__copy {
    text-align: center;
    p {
        display: flex;
        align-items: center;
        justify-content: center;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 20px;
        color: @secondaryLabel;
        cursor: pointer;
        margin: 0px;
        svg {
            margin-right: 5px;
        }
    }
  }
}
</style>
