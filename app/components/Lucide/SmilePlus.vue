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

const faceVariants = {
  normal: {
    scale: 1,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  },
  animate: {
    scale: 1.1,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  }
};

const plusVariants = {
  normal: {
    rotate: 0,
    scale: 1,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  },
  animate: {
    rotate: 90,
    scale: 1.2,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20,
      delay: 0.1
    }
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
        :variants="faceVariants"
        :animate="currentState"
        d="M22 11v1a10 10 0 1 1-9-10"
      />
      <motion.path
        :variants="faceVariants"
        :animate="currentState"
        d="M8 14s1.5 2 4 2 4-2 4-2"
      />
      <line x1="9" x2="9.01" y1="9" y2="9" />
      <line x1="15" x2="15.01" y1="9" y2="9" />
      <motion.path
        :variants="plusVariants"
        :animate="currentState"
        d="M16 5h6"
      />
      <motion.path
        :variants="plusVariants"
        :animate="currentState"
        d="M19 2v6"
      />
    </svg>
  </div>
</template> 