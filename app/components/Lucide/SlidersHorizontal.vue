<script setup lang="ts">
import { motion } from 'motion-v';

interface Props {
  size?: number;
  class?: string;
}

const props = withDefaults(defineProps<Props>(), {
  size: 28,
  class: '',
});

const emit = defineEmits<{
  startAnimation: [];
  stopAnimation: [];
}>();

const defaultTransition = {
  type: 'spring',
  stiffness: 100,
  damping: 12,
  mass: 0.4,
};

const isControlled = ref(false);
const currentState = ref('normal');

const startAnimation = () => {
  currentState.value = 'animate';
};

const stopAnimation = () => {
  currentState.value = 'normal';
};

const handleMouseEnter = () => {
  if (!isControlled.value) {
    startAnimation();
  } else {
    emit('startAnimation');
  }
};

const handleMouseLeave = () => {
  if (!isControlled.value) {
    stopAnimation();
  } else {
    emit('stopAnimation');
  }
};

defineExpose({
  startAnimation,
  stopAnimation,
});
</script>

<template>
  <div
    :class="[
      'cursor-pointer select-none p-2 hover:bg-accent rounded-md transition-colors duration-200 flex items-center justify-center',
      props.class,
    ]"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
  >
    <svg
      xmlns="http://www.w3.org/2000/svg"
      :width="size"
      :height="size"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
    >
      <motion.line
        x1="21"
        x2="14"
        y1="4"
        y2="4"
        :initial="false"
        :variants="{
          normal: { x2: 14 },
          animate: { x2: 10 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="10"
        x2="3"
        y1="4"
        y2="4"
        :variants="{
          normal: { x1: 10 },
          animate: { x1: 5 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="21"
        x2="12"
        y1="12"
        y2="12"
        :variants="{
          normal: { x2: 12 },
          animate: { x2: 18 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="8"
        x2="3"
        y1="12"
        y2="12"
        :variants="{
          normal: { x1: 8 },
          animate: { x1: 13 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="3"
        x2="12"
        y1="20"
        y2="20"
        :variants="{
          normal: { x2: 12 },
          animate: { x2: 4 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="16"
        x2="21"
        y1="20"
        y2="20"
        :variants="{
          normal: { x1: 16 },
          animate: { x1: 8 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="14"
        x2="14"
        y1="2"
        y2="6"
        :variants="{
          normal: { x1: 14, x2: 14 },
          animate: { x1: 9, x2: 9 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="8"
        x2="8"
        y1="10"
        y2="14"
        :variants="{
          normal: { x1: 8, x2: 8 },
          animate: { x1: 14, x2: 14 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
      <motion.line
        x1="16"
        x2="16"
        y1="18"
        y2="22"
        :variants="{
          normal: { x1: 16, x2: 16 },
          animate: { x1: 8, x2: 8 },
        }"
        :animate="currentState"
        :transition="defaultTransition"
      />
    </svg>
  </div>
</template>
