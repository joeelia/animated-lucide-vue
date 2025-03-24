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

const cloudVariants = {
  normal: {
    x: 0,
    y: 0,
  },
  animate: {
    x: [-1, 1, -1, 1, 0],
    y: [-1, 1, -1, 1, 0],
    transition: {
      duration: 1,
      ease: 'easeInOut',
    },
  },
};

const sunVariants = {
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

const sunRays = [
  'M12 2v2',
  'm4.93 4.93 1.41 1.41',
  'M20 12h2',
  'm19.07 4.93-1.41 1.41',
  'M15.947 12.65a4 4 0 0 0-5.925-4.128',
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
      style="overflow: visible"
    >
      <motion.g
        :variants="cloudVariants"
        :animate="currentState"
      >
        <path d="M13 22H7a5 5 0 1 1 4.9-6H13a3 3 0 0 1 0 6Z" />
      </motion.g>
      <motion.path
        v-for="(d, index) in sunRays"
        :key="d"
        :d="d"
        :variants="sunVariants"
        :animate="currentState"
        :custom="index + 1"
      />
    </svg>
  </div>
</template> 