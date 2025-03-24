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

const sunVariants = {
  normal: {
    rotate: 0,
  },
  animate: {
    rotate: [0, -5, 5, -2, 2, 0],
    transition: {
      duration: 1.5,
      ease: 'easeInOut',
    },
  },
};

const moonVariants = {
  normal: { opacity: 1 },
  animate: (i: unknown) => ({
    opacity: [0, 1],
    transition: { delay: Number(i) * 0.1, duration: 0.3 },
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

const rays = [
  'M12 2v2',
  'M12 20v2',
  'm4.9 4.9 1.4 1.4',
  'm17.7 17.7 1.4 1.4',
  'M2 12h2',
  'M20 12h2',
  'm6.3 17.7-1.4 1.4',
  'm19.1 4.9-1.4 1.4',
];
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
        :variants="sunVariants"
        :animate="currentState"
      >
        <path d="M12 8a2.83 2.83 0 0 0 4 4 4 4 0 1 1-4-4" />
      </motion.g>
      <motion.path
        v-for="(d, index) in rays"
        :key="d"
        :d="d"
        :variants="moonVariants"
        :animate="currentState"
        :custom="index + 1"
      />
    </svg>
  </div>
</template> 