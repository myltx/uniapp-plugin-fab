<!--
 * @Date: 2019-08-22 19:41:20
 * @LastEditTimes: Do not edit
 * @Descripttion: describe
-->
<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">Fab 悬浮按钮示例</text>
    </view>

    <view style="margin-top: 20rpx; width: 100%">
      <button type="primary" block>circle 布局</button>
    </view>
    <view style="margin-top: 20rpx; width: 100%">
      <button type="primary">column 布局</button>
    </view>

    <fab :menuList="menuList" :layout="'circle'" :position="[0, 0]">
      <template #menu-item="{ item }"> 12313 </template>
    </fab>
  </view>
</template>

<script>
import Fab from "@/components/fab/index.vue";

export default {
  components: {
    Fab,
  },
  data() {
    return {
      menuList: [
        {
          icon: "/static/logo.png",
          text: "拍照",
          type: "camera",
        },
        {
          icon: "/static/logo.png",
          text: "相册",
          type: "album",
        },
        {
          icon: "/static/logo.png",
          text: "用户",
          type: "user",
        },
      ],
    };
  },
  methods: {
    handleFabClick(item) {
      uni.showToast({
        title: `点击了${item.text}按钮`,
        icon: "none",
      });
    },
  },
};
</script>

<style lang="scss">
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  // background: #f7f7f7;
  // height: 80vh;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin: 200rpx auto 50rpx auto;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
