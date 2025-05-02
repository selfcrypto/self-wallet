<template>
  <div class="network-activity__action">
    <div class="network-activity__action-wrap">
			<a class="network-activity__action-item" @click="$emit('showSidebarMenu')">
					<Networks />Netowkrs
			</a>
			<a class="network-activity__action-item" >
					<Nft />NFT
			</a>
			<router-link
					:to="{
					name: 'send-transaction',
					params: { id: route.params.id, isToken: 'true' },
					}"
					class="network-activity__action-item"
			>
					<Send />Send
			</router-link>
			<a class="network-activity__action-item" @click="$emit('toggle:deposit')">
					<Deposit />Deposit
			</a>
      <!-- <a class="network-activity__action-item" @click="$emit('open:buyAction')">
        <Buy />Buy/Sell
      </a> -->
      <!-- <div class="network-activity__action-divider" /> -->
      <!-- <div class="network-activity__action-divider" /> -->
      <!-- <router-link
        class="network-activity__action-item"
        :to="{
          name: 'swap',
          params: { id: route.params.id },
        }"
      >
        <Swap />Swap
      </router-link> -->
    </div>
  </div>
</template>

<script setup lang="ts">
import Deposit from "@action/icons/actions/deposit.vue";
import Nft from "@action/icons/actions/nft.vue";
import Send from "@action/icons/actions/send.vue";
import Networks from "@action/icons/actions/networks.vue";
import { useRoute } from "vue-router";
import AppMenuContainer from "@action/components/app-menu-container/index.vue";
const route = useRoute();

defineEmits<{
  (e: "toggle:deposit"): void;
  (e: "open:buyAction"): void;
  (e: "showSidebarMenu"): void;
}>();
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.network-activity {
  &__action {
    padding-bottom: 11px;
    box-sizing: border-box;

    &-wrap {
      width: 100%;
      left: 12px;
      top: 0px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      flex-direction: row;
      box-sizing: border-box;
			column-gap: 8px;
    }
    &-item {
      display: block;
      text-align: center;
      text-decoration: none;
      font-style: normal;
      font-weight: 400;
      font-size: 14px;
      line-height: 20px;
      text-align: center;
      letter-spacing: 0px;
      color: @primaryLabel;
      cursor: pointer;
      height: 75px;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      width: 100%;
      transition: background 300ms ease-in-out;
      border-radius: 6px;
			background: @white;

      &:hover {
        background: rgba(0, 0, 0, 0.04);
      }

      svg {
        margin-bottom: 5px;
      }
    }
    &-divider {
      height: 48px;
      width: 1px;
      background: @darkBg;
      margin: 0 4px 0 4px;
    }
  }
}
</style>
