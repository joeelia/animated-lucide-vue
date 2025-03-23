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

const squareVariants = {
  normal: { transition: { duration: 0.4 } },
  animate: { transition: { duration: 0.6, ease: 'easeInOut' } },
};

const pathVariants = {
  normal: { d: 'm8 12 4 4 4-4', translateY: 0, opacity: 1 },
  animate: {
    d: 'm8 12 4 4 4-4',
    translateY: [0, -3, 0],
    transition: { duration: 0.4 },
  },
};

const secondPathVariants = {
  normal: { d: 'M12 8v8', opacity: 1 },
  animate: {
    d: ['M12 8v8', 'M12 8v5', 'M12 8v8'],
    transition: { duration: 0.4 },
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
      <motion.rect
        width="18"
        height="18"
        x="3"
        y="3"
        rx="2"
        :variants="squareVariants"
        :animate="currentState"
      />
      <motion.path
        :variants="pathVariants"
        :animate="currentState"
        d="m8 12 4 4 4-4"
      />
      <motion.path
        :variants="secondPathVariants"
        :animate="currentState"
        d="M12 8v8"
      />
    </svg>
  </div>
</template> 