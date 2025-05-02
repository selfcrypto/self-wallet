<template>
  <div class="wallet-ready">
    <div class="wallet-ready__top">
        <logo-big class="wallet-ready__logo" />
        <h3>Your wallet is ready</h3>
        <div class="wallet-ready__social">
        <h4>Follow us</h4>
        <div class="wallet-ready__social-wrap">
            <a href="https://github.com/myetherwallet" target="_blank">
            <github-icon />
            </a>
            <a href="https://www.instagram.com/myetherwallet/" target="_blank">
            <instagram-icon />
            </a>
            <a href="https://www.reddit.com/r/MyEtherWallet/" target="_blank">
            <reddit-icon />
            </a>
            <a href="https://twitter.com/myetherwallet" target="_blank">
            <twitter-icon />
            </a>
        </div>
        </div>
    </div>
    <div class="wallet-ready__bottom">
        <base-button title="Finish" :click="finishAction" />
    </div>
  </div>
</template>
<script setup lang="ts">
import BaseButton from "@action/components/base-button/index.vue";
import LogoBig from "@action/icons/common/logo-icon.vue";
import GithubIcon from "@action/icons/social/github-icon.vue";
import InstagramIcon from "@action/icons/social/instagram-icon.vue";
import RedditIcon from "@action/icons/social/reddit-icon.vue";
import TwitterIcon from "@action/icons/social/twitter-icon.vue";

const finishAction = () => {
  window.close();
};
</script>

<style lang="less">
@import "~@action/styles/theme.less";

.wallet-ready {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
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
    color: @gray03;
    margin: 0 0 21px 0;
    opacity: 0.9;
  }

  &__social {
    width: 100%;
    text-align: center;
    h4 {
      font-style: normal;
      font-weight: 400;
      font-size: 16px;
      line-height: 24px;
      color: @gray03;
      margin: 0 0 30px 0;
      opacity: 0.9;
    }

    &-wrap {
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: center;
      column-gap: 18px;
      a {
        display: block;
        svg {
            width: 24px
        }
        svg,
        path {
            color: @gray03;
            fill: @gray03;
        }
      }
    }
  }
}
</style>
