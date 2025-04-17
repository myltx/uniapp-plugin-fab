<!--
 * @Date: 2019-08-22 19:41:20
 * @LastEditTimes: Do not edit
 * @Descripttion: describe
-->
<template>
  <view class="content">
    <view class="text-area">
      <text class="title">Fab Circle 悬浮按钮示例</text>
    </view>
    <view class="tips">
      由于 Circle
      模式下，菜单项过多会导致按钮重叠，所以建议菜单项限制了最大为4个,如有需要可以在组件中自行调整
    </view>
    <view class="uni-title">是否可以拖动</view>
    <switch @change="switchChange" :checked="draggable" />
    <view class="uni-title">是否自动吸附</view>
    <switch @change="switch2Change" :checked="autosorption" />
    <view class="uni-title">是否使用插槽</view>
    <switch @change="switch3Change" :checked="useSlot" />
    <Fab
      :menuList="menuList"
      :layout="'circle'"
      :draggable="draggable"
      :autosorption="autosorption"
      :position="[350, 999]"
      @select="select">
      <template #menu-item="item" v-if="useSlot"> circle </template>
    </Fab>
  </view>
</template>

<script>
import Fab from "@/components/myl-uniapp-fab/index.vue";

export default {
  components: {
    Fab,
  },
  data() {
    return {
      autosorption: true,
      draggable: true,
      useSlot: false,
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
        {
          icon: "/static/logo.png",
          text: "用户",
          type: "user",
        },
      ],
    };
  },
  methods: {
    select(data) {
      console.log(data, "dd");
    },
    switchChange(e) {
      console.log("switch 发生 change 事件，携带值为", e.detail.value);
      this.draggable = e.detail.value;
    },
    switch2Change(e) {
      console.log("switch2 发生 change 事件，携带值为", e.detail.value);
      this.autosorption = e.detail.value;
    },
    switch3Change(e) {
      console.log("switch3 发生 change 事件，携带值为", e.detail.value);
      this.useSlot = e.detail.value;
    },
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
.tips {
  font-size: 24rpx;
  color: #8f8f94;
  margin: 10px 0;
}
.title {
  font-size: 36rpx;
  //   color: #8f8f94;
  font-weight: bold;
}
</style>
