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
  'M13.5 3.1c-.5 0-1-.1-1.5-.1s-1 .1-1.5.1',
  'M19.3 6.8a10.45 10.45 0 0 0-2.1-2.1',
  'M20.9 13.5c.1-.5.1-1 .1-1.5s-.1-1-.1-1.5',
  'M17.2 19.3a10.45 10.45 0 0 0 2.1-2.1',
  'M10.5 20.9c.5.1 1 .1 1.5.1s1-.1 1.5-.1',
  'M3.5 17.5 2 22l4.5-1.5',
  'M3.1 10.5c0 .5-.1 1-.1 1.5s.1 1 .1 1.5',
  'M6.8 4.7a10.45 10.45 0 0 0-2.1 2.1',
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