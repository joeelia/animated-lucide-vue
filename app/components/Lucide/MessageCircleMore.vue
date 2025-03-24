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

const dotVariants = {
  normal: {
    opacity: 1,
  },
  animate: (custom: unknown) => {
    const customNum = custom as number;
    return {
      opacity: [1, 0, 0, 1, 1, 0, 0, 1],
      transition: {
        opacity: {
          times: [
            0,
            0.1,
            0.1 + customNum * 0.1,
            0.1 + customNum * 0.1 + 0.1,
            0.5,
            0.6,
            0.6 + customNum * 0.1,
            0.6 + customNum * 0.1 + 0.1,
          ],
          duration: 1.5,
        },
      },
    };
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
      <path d="M7.9 20A9 9 0 1 0 4 16.1L2 22Z" />
      <motion.path
        d="M8 12h.01"
        :variants="dotVariants"
        :animate="currentState"
        :custom="0"
      />
      <motion.path
        d="M12 12h.01"
        :variants="dotVariants"
        :animate="currentState"
        :custom="1"
      />
      <motion.path
        d="M16 12h.01"
        :variants="dotVariants"
        :animate="currentState"
        :custom="2"
      />
    </svg>
  </div>
</template> 