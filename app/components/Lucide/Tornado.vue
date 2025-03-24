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

const pathVariants = {
  normal: {
    x: 0,
    opacity: 1,
    transition: {
      duration: 0.3,
      ease: 'easeInOut',
    },
  },
  animate: (custom: unknown) => ({
    x: [0, (custom as number) * 1, 0],
    opacity: 1,
    transition: {
      x: {
        duration: 0.6,
        repeat: 0.7,
        repeatType: 'reverse',
        ease: 'easeInOut',
        delay: (custom as number) * 0.1,
      },
    },
  }),
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
        d="M21 4H3"
        :variants="pathVariants"
        :animate="currentState"
        :custom="1"
      />
      <motion.path
        d="M18 8H6"
        :variants="pathVariants"
        :animate="currentState"
        :custom="2"
      />
      <motion.path
        d="M19 12H9"
        :variants="pathVariants"
        :animate="currentState"
        :custom="3"
      />
      <motion.path
        d="M16 16h-6"
        :variants="pathVariants"
        :animate="currentState"
        :custom="4"
      />
      <motion.path
        d="M11 20H9"
        :variants="pathVariants"
        :animate="currentState"
        :custom="5"
      />
    </svg>
  </div>
</template> 