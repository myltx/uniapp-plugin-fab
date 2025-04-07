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
      :style="getItemStyle(index)"
      @tap="handleSelect(item)">
      <slot name="menu-item" :item="item">
        <text class="menu-icon">{{ item.icon }}</text>
      </slot>
    </view>
  </view>
</template>

<script setup>
import { defineProps, defineEmits, computed } from "vue";

const props = defineProps({
  menuItems: {
    type: Array,
    required: true,
  },
  isOpen: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["select"]);

const getItemStyle = (index) => {
  let x = 0;
  let y = 0;
  let delay = 0.05 * index;
  console.log(index, "index");
  if (index === 0) {
    x = 60;
    y = 50;
    delay = 0;
  } else if (index === 1) {
    x = -30;
    y = 100;
    delay = 0.1;
  } else if (index === 2) {
    x = -30;
    y = 200;
    delay = 0.2;
  } else if (index === 3) {
    x = 60;
    y = 280;
  }

  console.log(x, y, "x, y");
  return {
    "--x": `${x}rpx`,
    "--y": `${y}rpx`,
    transitionDelay: `${delay}s`,
  };
};

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
  pointer-events: none;

  &.circle-menu-active {
    opacity: 1;
    pointer-events: auto;
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
