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

const DURATION = 0.25;

const calculateDelay = (i: unknown) => {
  return Number(i) === 0 ? 0.1 : Number(i) * DURATION + 0.1;
};

const pathVariants = {
  normal: {
    pathLength: 1,
    pathOffset: 0,
    opacity: 1,
    transition: { delay: 0 },
  },
  animate: {
    pathOffset: [1, 0],
    pathLength: [0, 1],
    opacity: [0, 1],
  },
};

const circleVariants = {
  normal: { pathLength: 1, opacity: 1, transition: { delay: 0 } },
  animate: {
    pathLength: [0, 1],
    opacity: [0, 1],
  },
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
        d="M11 2v2"
        :variants="pathVariants"
        :animate="currentState"
        :transition="{
          duration: DURATION,
          delay: calculateDelay(2),
          opacity: { delay: calculateDelay(2) },
        }"
      />
      <motion.path
        d="M5 2v2"
        :variants="pathVariants"
        :animate="currentState"
        :transition="{
          duration: DURATION,
          delay: calculateDelay(2),
          opacity: { delay: calculateDelay(2) },
        }"
      />
      <motion.path
        d="M5 3H4a2 2 0 0 0-2 2v4a6 6 0 0 0 12 0V5a2 2 0 0 0-2-2h-1"
        :variants="pathVariants"
        :animate="currentState"
        :transition="{
          duration: DURATION,
          delay: calculateDelay(2),
          opacity: { delay: calculateDelay(2) },
        }"
      />
      <motion.path
        d="M8 15a6 6 0 0 0 12 0v-3"
        :variants="pathVariants"
        :animate="currentState"
        :transition="{
          duration: DURATION,
          delay: calculateDelay(1),
          opacity: { delay: calculateDelay(1) },
        }"
      />
      <motion.circle
        cx="20"
        cy="10"
        r="2"
        :variants="circleVariants"
        :animate="currentState"
        :transition="{
          duration: DURATION,
          delay: calculateDelay(0),
          opacity: { delay: calculateDelay(0) },
        }"
      />
    </svg>
  </div>
</template> 