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

const windVariants = {
  normal: (custom: unknown) => ({
    pathLength: 1,
    opacity: 1,
    pathOffset: 0,
    transition: {
      duration: 0.3,
      ease: 'easeInOut',
      delay: custom as number,
    },
  }),
  animate: (custom: unknown) => ({
    pathLength: [0, 1],
    opacity: [0, 1],
    pathOffset: [1, 0],
    transition: {
      duration: 0.5,
      ease: 'easeInOut',
      delay: custom as number,
    },
  }),
};

const arrowVariants = {
  normal: {
    y: 0,
    opacity: 1,
    transition: {
      duration: 0.3,
      ease: 'easeInOut',
    },
  },
  animate: {
    y: [-10, 0],
    opacity: [0, 1],
    transition: {
      duration: 0.5,
      ease: 'easeInOut',
      delay: 0.35,
    },
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
      <!-- Wind paths -->
      <motion.path
        d="M12.8 21.6A2 2 0 1 0 14 18H2"
        :variants="windVariants"
        :animate="currentState"
        :custom="0.2"
      />
      <motion.path
        d="M17.5 10a2.5 2.5 0 1 1 2 4H2"
        :variants="windVariants"
        :animate="currentState"
        :custom="0.4"
      />
      <motion.path
        d="M10 2v8"
        :variants="arrowVariants"
        :animate="currentState"
      />
      <motion.path
        d="m6 6 4 4 4-4"
        :variants="arrowVariants"
        :animate="currentState"
      />
    </svg>
  </div>
</template> 