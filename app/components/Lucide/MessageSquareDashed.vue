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
  normal: { opacity: 1 },
  animate: (i: unknown) => ({
    opacity: [0, 1],
    transition: { 
      delay: (i as number) * 0.1, 
      duration: 0.3 
    },
  }),
};

const paths = [
  'M14 3h1',
  'M14 17h1',
  'M10 17H7l-4 4v-7',
  'M9 3h1',
  'M19 3a2 2 0 0 1 2 2',
  'M3 9v1',
  'M21 9v1',
  'M21 14v1a2 2 0 0 1-2 2',
  'M5 3a2 2 0 0 0-2 2',
];

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
        v-for="(d, index) in paths"
        :key="d"
        :d="d"
        :animate="currentState"
        :variants="pathVariants"
        :custom="index + 1"
      />
    </svg>
  </div>
</template> 