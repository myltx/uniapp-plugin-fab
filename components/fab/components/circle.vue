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
  const totalItems = props.menuItems.length;
  const angle = (360 / totalItems) * index;
  const radius = 150; // 圆形半径，单位rpx
  const delay = index * 0.05;

  return {
    transform: `rotate(${angle}deg) translate(${radius}rpx) rotate(-${angle}deg)`,
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
  width: 0;
  height: 0;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;

  &.circle-menu-active {
    opacity: 1;
    pointer-events: auto;
  }
}

.menu-item {
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80rpx;
  height: 80rpx;
  background-color: #ffffff;
  border-radius: 50%;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  transform-origin: center center;
  transform: scale(0.5) translate(0);
  opacity: 0;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);

  &.menu-item-active {
    transform: scale(1) translate(150rpx);
    opacity: 1;
  }
}

.menu-icon {
  font-size: 36rpx;
}
</style>
