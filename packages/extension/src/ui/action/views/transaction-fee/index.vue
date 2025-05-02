<template>
  <div class="transaction-fee" :class="{ show: showFees, popup: isPopup }">
    <div class="transaction-fee__overlay" @click="closepopup" />
    <div
      class="transaction-fee__wrap"
      :class="{ show: showFees, header: isHeader }"
    >
      <div v-if="isHeader" class="transaction-fee__header">
        <h3>Choose transaction fee</h3>

        <a class="transaction-fee__close" @click="closepopup">
          <close-icon />
        </a>
      </div>
      <div class="transaction-fee__info">
        <div class="transaction-fee__info-amount">
          <p class="transaction-fee__info-amount-fiat">
            ${{ $filters.formatFiatValue(fees[selected].fiatValue).value }}
          </p>
          <p class="transaction-fee__info-amount-crypto">
            {{
              $filters.formatFloatingPointValue(fees[selected].nativeValue)
                .value
            }}
            <span>{{ fees[selected].nativeSymbol }}</span>
          </p>
        </div>

        <div class="transaction-fee__info-text">
          This fee is charged by the network.
        </div>

        <div class="transaction-fee__info-time">
          <time-icon />
          <span>{{ FeeDescriptions[selected].eta }}</span>
        </div>
      </div>
      <transaction-fee-item
        v-for="(value, type) in fees"
        :key="type"
        :all-fees="fees"
        :selected="selected"
        :type="type"
        v-bind="$attrs"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { PropType } from "vue";
import TransactionFeeItem from "./components/transaction-fee-item.vue";
import TimeIcon from "@action/icons/fee/time-icon.vue";
import CloseIcon from "@action/icons/common/close-icon.vue";

import { GasFeeType, GasPriceTypes } from "@/providers/common/types";
import { FeeDescriptions } from "@/providers/ethereum/libs/transaction/gas-utils";
const emit = defineEmits<{
  (e: "closePopup"): void;
}>();

defineProps({
  showFees: Boolean,
  fees: {
    type: Object as PropType<GasFeeType>,
    default: () => ({}),
  },
  isHeader: {
    type: Boolean,
    default: () => {
      return false;
    },
  },
  isPopup: {
    type: Boolean,
    default: () => {
      return false;
    },
  },
  close: {
    type: Function,
    default: () => {
      return null;
    },
  },
  selectFee: {
    type: Function,
    default: () => {
      return null;
    },
  },
  selected: {
    type: String as PropType<GasPriceTypes>,
    default: GasPriceTypes.ECONOMY,
  },
});

const closepopup = () => {
  emit("closePopup");
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.transaction-fee {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 101;
  display: none;

  &.show {
    display: block;
  }

  &.popup {
    position: fixed;
  }

  &__overlay {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 102;
    background-color: @black;
    opacity: 0.8;
  }

  &__wrap {
    position: absolute;
    width: 100%;
    height: auto;
    max-height: 440px;
    left: 0px;
    bottom: 100px;
    background: @gray03;
    box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.039),
      0px 7px 24px rgba(0, 0, 0, 0.19);
    border-radius: 12px;
    z-index: 103;
    padding: 16px;
    box-sizing: border-box;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s, visibility 0s ease-in-out 0.3s;
    overflow: hidden;

    &.show {
      opacity: 1;
      visibility: visible;
      transition-delay: 0s;
    }

    &.header {
      padding: 16px;
      max-height: 512px;
      bottom: 0px;
    }
  }

  &__info {
    margin-bottom: 16px;
    padding: 0px;
    position: relative;

    &-amount {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: row;
      margin-bottom: 5px;
      &-fiat {
        font-style: normal;
        font-weight: 500;
        font-size: 16px;
        line-height: 20px;
        letter-spacing: 0px;
        color: @primaryLabel;
        margin: 0;
        margin-right: 8px;
      }

      &-crypto {
        font-style: normal;
        font-weight: 500;
        font-size: 16px;
        line-height: 20px;
        color: @secondaryLabel;
        margin: 0;

        span {
          font-variant: small-caps;
        }
      }
    }

    &-text {
      font-style: normal;
      font-weight: 400;
      font-size: 14px;
      line-height: 20px;
      color: @secondaryLabel;
      margin: 0;
    }

    &-time {
      position: absolute;
      right: 0px;
      top: 0px;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: row;

      span {
        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: 20px;
        color: @primaryLabel;
      }

      svg {
        margin-right: 4px;
      }
    }
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
