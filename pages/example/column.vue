<!--
 * @Date: 2019-08-22 19:41:20
 * @LastEditTimes: Do not edit
 * @Descripttion: describe
-->
<template>
  <view class="content">
    <view class="text-area">
      <text class="title">Fab Column 悬浮按钮示例</text>
    </view>
    <view class="tips">
      由于 Column 模式下菜单过多可能会导致按扭超出屏幕，所以在组件中设置了最多为
      6 项,如有需要可以在组件中自行调整
    </view>
    <view class="uni-title">是否可以拖动</view>
    <switch @change="switchChange" :checked="draggable" />
    <view class="uni-title">是否自动吸附</view>
    <switch @change="switch2Change" :checked="autosorption" />
    <view class="uni-title">弹出方向</view>
    <radio-group @change="radioChange" class="radio-group">
      <label
        class="uni-list-cell uni-list-cell-pd"
        v-for="(item, index) in items"
        :key="item.value">
        <view class="radio">
          <radio :value="item.value" :checked="index === current" />
        </view>
        <view>{{ item.name }}</view>
      </label>
    </radio-group>
    <view class="uni-title">是否使用插槽</view>
    <switch @change="switch3Change" :checked="useSlot" />
    <Fab
      :menuList="menuList"
      :layout="'column'"
      :draggable="draggable"
      :autosorption="autosorption"
      :direction="items[current].value"
      :position="[350, 999]">
      <template #menu-item="item" v-if="useSlot"> 123 </template>
    </Fab>
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
      autosorption: true,
      draggable: true,
      useSlot: false,
      current: 0,
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
      items: [
        {
          value: "top",
          name: "上",
          checked: "true",
        },
        {
          value: "bottom",
          name: "下",
        },
        {
          value: "left",
          name: "左",
        },
        {
          value: "right",
          name: "右",
        },
      ],
    };
  },
  methods: {
    switchChange(e) {
      this.draggable = e.detail.value;
    },
    switch2Change(e) {
      this.autosorption = e.detail.value;
    },
    switch3Change(e) {
      this.useSlot = e.detail.value;
    },
    radioChange: function (evt) {
      for (let i = 0; i < this.items.length; i++) {
        if (this.items[i].value === evt.detail.value) {
          this.current = i;
          break;
        }
      }
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
.radio-group {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.uni-list-cell {
  display: flex;
  margin-right: 10rpx;
}
.radio {
  margin-right: 10rpx;
}
</style>
