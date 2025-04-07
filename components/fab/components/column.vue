<!--
 * @Date: 2025-04-07 15:12:47
 * @LastEditTimes: Do not edit
 * @Descripttion: describe
-->
<template>
  <view
    class="column-menu"
    :class="{ 'column-menu-active': isOpen }"
    :style="menuStyle"
    :data-direction="direction">
    <view
      v-for="(item, index) in menuItems"
      :key="index"
      class="menu-item"
      :class="{ 'menu-item-active': isOpen }"
      :style="{ transitionDelay: `${index * 0.05}s` }"
      @tap="handleSelect(item)">
      <slot name="menu-item" :item="item">
        <text class="menu-icon">{{ item.icon }}</text>
        <text class="menu-text" v-if="item.text">{{ item.text }}</text>
      </slot>
    </view>
  </view>
</template>

<script setup>
import {
  defineProps,
  defineEmits,
  ref,
  onMounted,
  computed,
  watchEffect,
} from "vue";

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
    type: Object,
    default: () => [10, 100],
  },
  direction: {
    type: String,
    default: "up",
    validator: (value) => ["up", "down", "left", "right"].includes(value),
  },
});

const direction = ref(props.direction);
const emit = defineEmits(["select"]);
const menuRef = ref(null);
const windowHeight = ref(0);
const windowWidth = ref(0);

onMounted(() => {
  const systemInfo = uni.getSystemInfoSync();
  windowHeight.value = systemInfo.windowHeight;
  windowWidth.value = systemInfo.windowWidth;
});

const computeDirection = () => {
  let [left, top] = props.position;
  const itemHeight = 80; // 每个菜单项的高度（包含间距）
  const itemWidth = 200; // 菜单项的宽度
  const menuPadding = 20; // 菜单的内边距
  const menuHeight = props.menuItems.length * itemHeight + menuPadding * 2;
  const menuWidth = itemWidth + menuPadding * 2;
  const safeDistance = 20; // 边界安全距离

  // 计算按钮位置相对于屏幕边界的距离
  const distanceToTop = top;
  const distanceToBottom = windowHeight.value - top;
  const distanceToLeft = left;
  const distanceToRight = windowWidth.value - left;

  // 根据按钮位置自动选择最优展开方向
  if (["up", "down"].includes(direction.value)) {
    if (distanceToTop < menuHeight + safeDistance) {
      direction.value = "down";
    } else if (
      distanceToBottom < menuHeight + safeDistance &&
      distanceToTop > distanceToBottom
    ) {
      direction.value = "up";
    }
  } else if (["left", "right"].includes(direction.value)) {
    if (
      distanceToLeft < menuWidth + safeDistance &&
      distanceToRight > distanceToLeft
    ) {
      direction.value = "right";
    } else if (
      distanceToRight < menuWidth + safeDistance &&
      distanceToLeft > distanceToRight
    ) {
      direction.value = "left";
    }
  }
};

watchEffect(() => {
  props.isOpen && computeDirection();
});

const handleSelect = (item) => {
  emit("select", item);
};
</script>

<style lang="scss" scoped>
.column-menu {
  position: fixed;
  gap: 10rpx;
  // padding: 20rpx;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
  z-index: 999;
  display: flex;
  justify-content: center;

  &[data-direction="up"],
  &[data-direction="down"] {
    flex-direction: column;
    left: 18%;
    // transform: translate(0 -50%);
  }
  &[data-direction="up"] {
    bottom: 120rpx;
  }
  &[data-direction="down"] {
    top: 120rpx;
  }
  &[data-direction="left"],
  &[data-direction="right"] {
    flex-direction: row;
    transform: translate(0 -50%);
  }
  &[data-direction="left"] {
    right: 120rpx;
    top: 50%;
  }
  &[data-direction="right"] {
    left: 120rpx;
    top: 50%;
  }

  &.column-menu-active {
    opacity: 1;
    pointer-events: auto;
  }
}

.menu-item {
  display: flex;
  align-items: center;
  gap: 20rpx;
  padding: 20rpx;
  background-color: #ffffff;
  border-radius: 16rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
  transform: scale(0.5);
  opacity: 0;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);

  &.menu-item-active {
    transform: scale(1);
    opacity: 1;
  }
}

.menu-icon {
  font-size: 40rpx;
}

.menu-text {
  font-size: 28rpx;
  color: #333;
}
</style>
