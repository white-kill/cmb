<template>
  <view class="app">
    <view class="custom-nav" :style="computedCustomNavStyle">
      <view class="nav-content" :style="{ height: `${navBarHeight}px` }">
        <view :style="{ height: `${navBarHeight}px` }" class="nav-back">
          <image
            class="nav-back-icon"
            :src="
              navIndex == 0 || navIndex == 1
                ? '/static/icon/back.png'
                : '/static/icon/return@2x.png'
            "
            @click="goBack"
          />
        </view>
        <view
          class="nav-center"
          :class="`nav-center-${navIndex}`"
          :style="{ height: `${navBarHeight}px` }"
        >
          <view :class="navIndex == 0 ? 'active' : ''" @click="onNavClick(0)"
            >大众卡</view
          >
          <view :class="navIndex == 1 ? 'active' : ''" @click="onNavClick(1)"
            >金卡</view
          >
          <view :class="navIndex == 2 ? 'active' : ''" @click="onNavClick(2)"
            >金葵花</view
          >
          <view :class="navIndex == 3 ? 'active' : ''" @click="onNavClick(3)"
            >私人银行</view
          >
        </view>
        <view
          :style="{ height: `${navBarHeight}px` }"
          class="nav-back nav-more"
        >
          <image
            class="nav-back-icon"
            @click="showMore = true"
            :src="
              navIndex == 0 || navIndex == 1
                ? '/static/icon/icon3.png'
                : '/static/icon/icon3-w.png'
            "
          />
        </view>
      </view>
    </view>
    <u-swiper
      :list="list"
      :current="type"
      :height="computedSwiperHeight"
      @change="onSwiperChange"
      :autoplay="false"
      :radius="0"
    ></u-swiper>
    <image
      class="page-image"
      :src="`/static/pages/member-content-${navIndex + 1}.png`"
      mode="widthFix"
    ></image>
    <view class="more-overlay" v-show="showMore" @click="showMore = false">
      <image
        :style="{ top: `${navBarHeight/2 + statusBarHeight + 10}px` }"
        class="nav-more-content"
        :src="`/static/home/nav-more-content.png`"
        mode="widthFix"
      ></image>
    </view>
  </view>
</template>

<script>
import { mapState, mapActions } from "vuex";
import { navigateBack } from "@/utils/index.js";
export default {
  data() {
    return {
      opacity: 1,
      navIndex: 0,
      type: 0,
      showMore: false,
      list: [
        "/static/home/member-1-1.png",
        "/static/home/member-1-2.png",
        "/static/home/member-2-1.png",
        "/static/home/member-2-2.png",
        "/static/home/member-3-1.png",
        "/static/home/member-3-2.png",
        "/static/home/member-3-3.png",
        "/static/home/member-4-1.png",
      ],
      swiperHeight: 100,
    };
  },
  computed: {
    ...mapState(["navBarHeight", "statusBarHeight", "userInfo"]),
    computedCustomNavStyle() {
      if (this.navIndex == 0 || this.navIndex == 1) {
        return {
          paddingTop: `${this.statusBarHeight}px`,
          backgroundColor: `rgba(255,255,255,${1 - this.opacity})`,
        };
      }
      return {
        paddingTop: `${this.statusBarHeight}px`,
        backgroundColor: `rgba(0,0,0,${1 - this.opacity})`,
      };
    },
    computedSwiperHeight() {
      const windowWidth = uni.getSystemInfoSync().windowWidth;
      return (windowWidth / 1080) * 700;
    },
  },
  onLoad() {
    this.get_user_info();
  },
  onPageScroll(e) {
    var scrollTop = e.scrollTop;
    if (scrollTop == 0) {
      this.opacity = 1;
      return;
    } else if (scrollTop >= this.statusBarHeight) {
      this.opacity = 0;
      return;
    }
    this.opacity = 1 - scrollTop / this.statusBarHeight;
  },
  methods: {
    ...mapActions(["get_user_info"]),
    goBack() {
      navigateBack();
    },
    onNavClick(index) {
      if (this.navIndex != index) {
        this.navIndex = index;
        if (index == 0) this.type = 0;
        if (index == 1) this.type = 2;
        if (index == 2) this.type = 4;
        if (index == 3) this.type = 7;
      }
    },
    onSwiperChange(e) {
      this.type = e.current;
      if (e.current == 0 || e.current == 1) this.navIndex = 0;
      else if (e.current == 2 || e.current == 3) this.navIndex = 1;
      else if (e.current == 4 || e.current == 5 || e.current == 6)
        this.navIndex = 2;
      else if (e.current == 7) this.navIndex = 3;
    },
  },
};
</script>

<style lang="scss" scoped>
.app {
  width: 100%;
  min-height: 100vh;
  background-color: #f8f8f8;
  position: relative;
}
.custom-nav {
  position: fixed;
  top: 0;
  background-color: red;
  z-index: 1;
  width: 750rpx;

  .nav-content {
    display: flex;
    justify-content: space-between;

    .nav-back {
      position: absolute;
      left: 34rpx;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 50rpx;
      box-sizing: border-box;
      height: 100%;
      z-index: 10;

      .nav-back-icon {
        width: 50rpx;
        height: 50rpx;
      }
    }

    .nav-more {
      right: 34rpx;
      left: auto;
    }

    .nav-center {
      flex: 1;
      display: flex;
      gap: 20rpx;
      align-items: center;
      justify-content: center;
      color: #517b9c;
      font-size: 29rpx;
    }

    .nav-center-0 {
      color: #448aac;
      .active {
        color: #036890;
        position: relative;
      }
      .active::after {
        content: "";
        position: absolute;
        bottom: -20rpx;
        width: 40rpx;
        left: calc(50% - 20rpx);
        right: 0;
        height: 6rpx;
        border-radius: 3rpx;
        background-color: #036890;
      }
    }

    .nav-center-1 {
      color: #9f7025;
      .active {
        color: #774000;
        position: relative;
      }
      .active::after {
        content: "";
        position: absolute;
        bottom: -20rpx;
        width: 40rpx;
        left: calc(50% - 20rpx);
        right: 0;
        height: 6rpx;
        border-radius: 3rpx;
        background-color: #774000;
      }
    }

    .nav-center-2 {
      color: #c3b1a3;
      .active {
        color: #ffefdb;
        position: relative;
      }
      .active::after {
        content: "";
        position: absolute;
        bottom: -20rpx;
        width: 40rpx;
        left: calc(50% - 20rpx);
        right: 0;
        height: 6rpx;
        border-radius: 3rpx;
        background-color: #ffefdb;
      }
    }

    .nav-center-3 {
      color: #b7aca1;
      .active {
        color: #ffefde;
        position: relative;
      }
      .active::after {
        content: "";
        position: absolute;
        bottom: -20rpx;
        width: 40rpx;
        left: calc(50% - 20rpx);
        right: 0;
        height: 6rpx;
        border-radius: 3rpx;
        background-color: #ffefde;
      }
    }
  }
}
.more-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0);
  z-index: 10;
}
.nav-more-content {
  position: absolute !important;
  width: 250rpx;
  right: 32rpx;
}
.page-image {
  width: 750rpx;
  height: auto;
}
image {
  display: flex;
}
</style>
