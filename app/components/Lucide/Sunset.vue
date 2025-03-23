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

const arrowVariants = {
  normal: {
    y: 0,
  },
  animate: {
    y: [0, 1, 0],
  },
};

const raysVariants = {
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
  'm4.93 10.93 1.41 1.41',
  'M2 18h2',
  'M20 18h2',
  'm19.07 10.93-1.41 1.41',
  'M22 22H2',
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
        :variants="arrowVariants"
        :animate="currentState"
      >
        <path d="M12 10V2" />
        <path d="m16 6-4 4-4-4" />
      </motion.g>

      <motion.path
        v-for="(d, index) in rays"
        :key="d"
        :d="d"
        :variants="raysVariants"
        :animate="currentState"
        :custom="index + 1"
      />
      <path d="M16 18a4 4 0 0 0-8 0" />
    </svg>
  </div>
</template> 