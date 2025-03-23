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

const arrowTransition = {
  type: 'spring',
  stiffness: 250,
  damping: 25,
};

const arrowVariants = {
  normal: {
    rotate: '0deg',
  },
  animate: {
    rotate: '-50deg',
  },
};

const handTransition = {
  duration: 0.6,
  ease: [0.4, 0, 0.2, 1],
};

const handVariants = {
  normal: {
    rotate: 0,
    originX: '50%',
    originY: '50%',
  },
  animate: {
    rotate: -360,
  },
};

const minuteHandTransition = {
  duration: 0.5,
  ease: 'easeInOut',
};

const minuteHandVariants = {
  normal: {
    rotate: 0,
    originX: '50%',
    originY: '50%',
  },
  animate: {
    rotate: -45,
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
      <motion.g
        :transition="arrowTransition"
        :variants="arrowVariants"
        :animate="currentState"
      >
        <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8" />
        <path d="M3 3v5h5" />
      </motion.g>
      <motion.line
        x1="12"
        y1="12"
        x2="12"
        y2="7"
        :variants="handVariants"
        :animate="currentState"
        :transition="handTransition"
      />
      <motion.line
        x1="12"
        y1="12"
        x2="16"
        y2="14"
        :variants="minuteHandVariants"
        :animate="currentState"
        :transition="minuteHandTransition"
      />
    </svg>
  </div>
</template> 