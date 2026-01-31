<template>
  <view class="app">
    <navbar title="交易查询" :ai-btn="true" :more-btn='true'></navbar>
    <view class="pages">
      <image
        class="page-image"
        src="/static/pages/jycx_1.png"
        mode="widthFix"
      ></image>
      <view class="card-number">{{ userInfo.bankList[0].bankCard.slice(-4) }}</view>
    </view>
  </view>
</template>

<script>
import { navigateBack } from "@/utils/index.js";
import {
  mapState,
 } from 'vuex'
export default {
  data() {
    return {
      details: {},
    };
  },
  computed: {
    ...mapState(['userInfo']),
  },
  methods: {
    goBack() {
      navigateBack();
    },
  },
};
</script>

<style lang="scss">
.app {
  width: 750rpx;
  min-height: 100vh;
}

.pages {
  width: 750rpx;
  height: auto;
  position: relative;

  .page-image {
    width: 750rpx;
    height: auto;
  }

  .card-number {
    position: absolute;
    top: 132rpx;
    right: 82rpx;
    text-align: center;
    font-size: 30rpx;
    color: #333333;
  }
}
</style>
