<!--
 * @Date: 2025-04-07 15:13:07
 * @LastEditTimes: Do not edit
 * @Descripttion: describe
-->
<template>
  <view class="circle-menu" :class="{ 'circle-menu-active': isOpen }">
    <view
      v-for="(item, index) in menuItems"
      :key="index"
      class="menu-item"
      :class="{ 'menu-item-active': isOpen }"
      :style="newItemStyle[index]"
      @tap="handleSelect(item)">
      <slot name="menu-item" :data="item" :index="index">
        <text class="menu-icon">{{ item.icon }}</text>
      </slot>
    </view>
  </view>
</template>

<script setup>
import { defineProps, defineEmits, ref, watchEffect } from "vue";
import { circlePositionConfig } from "./fab.config";

const props = defineProps({
  menuItems: {
    type: Array,
    required: true,
  },
  isOpen: {
    type: Boolean,
    default: false,
  },
  position: {
    type: Array,
    default: () => [10, 100],
  },
});

const emit = defineEmits(["select"]);
const direction = ref("left");

const getItemStyle = (index) => {
  const config = circlePositionConfig[direction.value];

  const { x, y, delay } = config[index] || config[0];

  return {
    "--x": `${x}rpx`,
    "--y": `${y}rpx`,
    transitionDelay: `${delay}s`,
  };
};

const newItemStyle = ref({
  0: getItemStyle(0),
  1: getItemStyle(1),
  2: getItemStyle(2),
  3: getItemStyle(3),
});

watchEffect(() => {
  if (props.isOpen) {
    let [x] = props.position;
    // 获取页面宽度
    const windowWidth = uni.getSystemInfoSync().windowWidth;
    if (windowWidth / 2 > x) {
      direction.value = "right";
    } else {
      direction.value = "left";
    }
    newItemStyle.value = {
      0: getItemStyle(0),
      1: getItemStyle(1),
      2: getItemStyle(2),
      3: getItemStyle(3),
    };
  }
});

const handleSelect = (item) => {
  emit("select", item);
};
</script>

<style lang="scss" scoped>
.circle-menu {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 200rpx;
  height: 400rpx;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;

  &.circle-menu-active {
    opacity: 1;
    pointer-events: none;
  }
}

.menu-item {
  position: absolute;
  // left: 0;
  // top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 70rpx;
  height: 70rpx;
  background-color: #ffffff;
  border-radius: 50%;
  box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.15);
  transform-origin: center center;
  transform: scale(0.5) translate(0);
  opacity: 0;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);

  &.menu-item-active {
    transform: scale(1) translate(var(--x, 0), var(--y, 0));
    opacity: 1;
  }
}

.menu-icon {
  font-size: 36rpx;
}
</style>
