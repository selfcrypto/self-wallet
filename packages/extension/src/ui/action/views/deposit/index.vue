<template>
  <div class="deposit" :class="{ show: showDeposit }">
    <div class="deposit__overlay" @click="$emit('toggle:deposit')" />
    <div class="deposit__wrap" :class="{ show: showDeposit }">
      <div class="deposit__header">
        <h3>Deposit</h3>
        <a class="deposit__close" @click="$emit('toggle:deposit')">
          <close-icon />
        </a>
      </div>
      <div class="deposit__logo">
        <img :src="network.icon" />
      </div>
      <h2>Your {{ network.name_long }} address</h2>
      <p>
        {{ depositCopy }}
      </p>

      <div class="deposit__code">
        <qrcode-vue
          :value="
            $props.network?.provider == ProviderName.kadena
              ? network.displayAddress(account.address)
              : network.provider + ':' + network.displayAddress(account.address)
          "
          :size="150"
          level="H"
        />
      </div>
      <div class="deposit__username">
        <p>@example</p>
      </div>
      <div class="deposit__account">
        <div class="deposit__account-detail">
          <img
            :src="network.identicon(network.displayAddress(account.address))"
          />

          <div class="deposit__account-info">
            <h4>{{ account.name }}</h4>
            <p>{{ network.displayAddress(account.address) }}</p>
          </div>
        </div>

        <a
          class="deposit__account-copy"
          @click="copy(network.displayAddress(account.address))"
        >
          <CopyIcon /><span>copy</span>
        </a>

        <notification
          v-if="isCopied"
          :hide="toggleNotification"
          text="Address copied"
          class="deposit__notification"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { PropType, ref, onMounted, computed } from "vue";
import CloseIcon from "@action/icons/common/close-icon.vue";
import CopyIcon from "@action/icons/header/copy_icon.vue";
import QrcodeVue from "qrcode.vue";
import { EnkryptAccount } from "@enkryptcom/types";
import Notification from "@action/components/notification/index.vue";
import { ProviderName } from "@/types/provider";
import { BaseNetwork, SubNetworkOptions } from "@/types/base-network";
import DomainState from "@/libs/domain-state";
const isCopied = ref(false);
const subNetwork = ref<SubNetworkOptions | null>(null);
const props = defineProps({
  network: {
    type: Object as PropType<BaseNetwork>,
    default: () => ({}),
  },
  account: {
    type: Object as PropType<EnkryptAccount>,
    default: () => {
      return {};
    },
  },
  showDeposit: {
    type: Boolean,
    default: () => false,
  },
});
defineEmits<{
  (e: "toggle:deposit"): void;
}>();
const copy = (address: string) => {
  navigator.clipboard.writeText(address);
  toggleNotification();
};

const toggleNotification = () => {
  isCopied.value = !isCopied.value;
};

onMounted(() => {
  if (props.network.subNetworks) {
    const domainState = new DomainState();
    domainState.getSelectedSubNetWork().then((id) => {
      const subnet = props.network.subNetworks?.find((net) => net.id === id);
      if (subnet) subNetwork.value = subnet;
    });
  }
});

const depositCopy = computed(() => {
  if (subNetwork.value !== null)
    return `You can send ${props.network.currencyNameLong} to this address in ${subNetwork.value.name} using ${props.network.name_long} network.`;
  else
    return `You can send ${props.network.currencyNameLong} to this address using ${props.network.name_long} network.`;
});
</script>

<style lang="less" scoped>
@import "~@action/styles/theme.less";

.deposit {
  width: 100%;
  height: 100%;
  left: 0px;
  top: 0px;
  position: fixed;
  z-index: 105;
  display: none;
  box-sizing: border-box;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  text-align: center;
  &.show {
    display: flex;
  }

  &__overlay {
    background: rgba(0, 0, 0, 0.32);
    width: 100%;
    height: 100%;
    left: 0px;
    top: 0px;
    position: absolute;
    z-index: 106;
  }

  &__header {
    width: 100%;
    box-sizing: border-box;
    padding: 0px;
    position: relative;
    z-index: 4;
    margin-bottom: 40px;

    h3 {
      font-style: normal;
      font-weight: 500;
      font-size: 20px;
      line-height: 24px;
      margin: 0;
      color: @primaryLabel;
    }
  }

  &__wrap {
    width: 100%;
    height: 100%;
    background: @gray03;
    box-shadow: 0px 0.5px 5px rgba(0, 0, 0, 0.039),
      0px 3.75px 11px rgba(0, 0, 0, 0.19);
    border-radius: 12px;
    position: relative;
    z-index: 107;
    overflow: hidden;
    padding: 16px;
    box-sizing: border-box;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s, visibility 0s ease-in-out 0.3s;
    &.show {
      opacity: 1;
      visibility: visible;
      transition-delay: 0s;
    }

    h2 {
      font-style: normal;
      font-weight: 500;
      font-size: 20px;
      line-height: 30px;
      color: @secondaryLabel;
      margin: 0;
    }

    p {
      font-style: normal;
      font-weight: 400;
      font-size: 16px;
      line-height: 24px;
      color: @secondaryLabel;
      margin: 0 0 20px 0;
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

  &__logo {
    width: 63px;
    height: 63px;
    background: linear-gradient(179.38deg, #5D70ED 0.54%, #354087 169.1%);
    border-radius: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0px auto 10px;
    padding: 10px;
    box-sizing: border-box;
    img {
      display: block;
      height: 100%;
      width: 100%;
    }
  }

  &__code {
    width: 230px;
    height: 210px;
    background: @white;
    border-radius: 6px;
    padding: 24px;
    box-sizing: border-box;
    border: 0.5px solid #CBCBCBB2;
    margin: 0px auto;
    box-shadow: 0px 0px 2.3px 11px #00000008 inset;
    img {
      max-width: 148px;
    }
  }

  &__account {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-direction: row;
    border: 0.5px solid #CBCBCBB2;
    padding: 7px 10px;
    border-radius: 4px;
    img {
      max-width: 21px;
      margin-right: 10px;
    }

    &-detail {
      display: flex;
      align-items: flex-start;
      justify-content: flex-start;
      flex: 1;
    }
    &-info {
      text-align: left;
      h4 {
        font-style: normal;
        font-weight: 400;
        font-size: 16px;
        line-height: 20px;
        color: @secondaryLabel;
        margin: 0;
        max-width: 60px;
        white-space: nowrap;
        -ms-text-overflow: ellipsis;
        -o-text-overflow: ellipsis;
        text-overflow: ellipsis;
        overflow: hidden;
      }

      p {
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 14px;
        color: @secondaryLabel;
        margin: 0;
        max-width: 190px;
        word-break: break-all;
        opacity: 0.5;
        white-space: nowrap;
        -ms-text-overflow: ellipsis;
        -o-text-overflow: ellipsis;
        text-overflow: ellipsis;
        overflow: hidden;
      }
    }

    &-copy {
      padding: 5px 12px;
      box-sizing: border-box;
      background: @white;
      border-radius: 4px;
      text-decoration: none;
      font-style: normal;
      font-size: 12px;
      line-height: 16px;
      text-transform: capitalize;
      color: @primaryLabel;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: row;
      cursor: pointer;
      transition: opacity 300ms ease-in-out;
      &:hover {
        opacity: 0.8;
      }

      svg {
        max-width: 16px;
        margin-right: 4px;
      }
    }
  }

  &__notification {
    position: absolute;
    left: 117px;
    bottom: 16px;
  }
  &__username {
    margin: 12px 0px;
    p {
      font-size: 14px;
      line-height: 20px;
      color: @primaryLabel;
      margin: 0px;
    }
  }
}
</style>
