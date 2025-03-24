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
  'M12 4h.01',
  'M20 12h.01',
  'M12 20h.01',
  'M4 12h.01',
  'M17.657 6.343h.01',
  'M17.657 17.657h.01',
  'M6.343 17.657h.01',
  'M6.343 6.343h.01',
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
      <circle cx="12" cy="12" r="4" />
      <motion.path
        v-for="(d, index) in rays"
        :key="d"
        :d="d"
        :variants="pathVariants"
        :animate="currentState"
        :custom="index + 1"
      />
    </svg>
  </div>
</template> 