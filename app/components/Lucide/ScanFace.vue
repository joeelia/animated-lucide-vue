<script setup lang="ts">
import { motion } from 'motion-v';
import { ref } from 'vue';

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
    scale: [1, 0.9],
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  }
};

const cornerVariants = {
  normal: {
    scale: 1,
    rotate: 0,
    opacity: 1,
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  },
  animate: {
    scale: [1, 1.2],
    rotate: [0, 45],
    opacity: [1, 0],
    transition: {
      type: 'spring',
      stiffness: 200,
      damping: 20
    }
  }
};

const mouthVariants = {
  normal: {
    scale: 1,
    opacity: 1,
    transition: {
      duration: 0.3
    }
  },
  animate: {
    scale: [1, 0.8],
    opacity: [1, 0],
    transition: {
      duration: 0.3,
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
        :variants="cornerVariants"
        :animate="currentState"
        d="M3 7V5a2 2 0 0 1 2-2h2"
      />
      <motion.path
        :variants="cornerVariants"
        :animate="currentState"
        d="M17 3h2a2 2 0 0 1 2 2v2"
      />
      <motion.path
        :variants="cornerVariants"
        :animate="currentState"
        d="M21 17v2a2 2 0 0 1-2 2h-2"
      />
      <motion.path
        :variants="cornerVariants"
        :animate="currentState"
        d="M7 21H5a2 2 0 0 1-2-2v-2"
      />
      <motion.path
        :variants="mouthVariants"
        :animate="currentState"
        d="M8 14s1.5 2 4 2 4-2 4-2"
      />
      <line x1="9" x2="9.01" y1="9" y2="9" />
      <line x1="15" x2="15.01" y1="9" y2="9" />
    </svg>
  </div>
</template> 