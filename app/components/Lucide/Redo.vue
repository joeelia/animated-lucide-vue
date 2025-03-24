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

// Custom easing function similar to cubicBezier in Framer Motion
const customEasing = [0.25, 0.1, 0.25, 1];

const firstPathVariants = {
  normal: { translateX: 0, translateY: 0, rotate: 0 },
  animate: {
    translateX: [0, -2.1, 0],
    translateY: [0, -1.4, 0],
    rotate: [0, -12, 0],
    transition: { duration: 0.6, ease: customEasing }
  }
};

const secondPathVariants = {
  normal: { pathLength: 1 },
  animate: { 
    pathLength: [1, 0.8, 1],
    transition: { duration: 0.6, ease: customEasing }
  }
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
      <motion.path
        d="M21 7v6h-6"
        :animate="currentState"
        :variants="firstPathVariants"
      />
      <motion.path
        d="M3 17a9 9 0 0 1 9-9 9 9 0 0 1 6 2.3l3 2.7"
        :animate="currentState"
        :variants="secondPathVariants"
      />
    </svg>
  </div>
</template> 