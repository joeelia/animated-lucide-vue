<script setup lang="ts">
import { motion } from 'motion-v'

interface Props {
  size?: number
  class?: string
}

const props = withDefaults(defineProps<Props>(), {
  size: 28,
  class: ''
})

const emit = defineEmits<{
  startAnimation: []
  stopAnimation: []
}>()

const isControlled = ref(false)
const currentState = ref('normal')

const faceVariants = {
  normal: {
    scale: 1,
    rotate: 0,
    strokeWidth: 2,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scale: [1, 1.15, 1, 1.1, 1.05],
    rotate: [0, 3, -2, 3, 0],
    strokeWidth: [2, 2.5, 2.5, 2.5, 2],
    transition: {
      duration: 1.2,
      times: [0, 0.2, 0.4, 0.6, 1],
      ease: 'easeInOut',
      repeat: 0,
      repeatType: 'reverse' as const
    }
  }
}

const mouthVariants = {
  normal: {
    d: 'M18 13a6 6 0 0 1-6 5 6 6 0 0 1-6-5h12Z',
    pathLength: 1,
    strokeWidth: 2,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    d: 'M18 13a6 6 0 0 1-6 5 6 6 0 0 1-6-5h12Z',
    pathLength: [0.7, 1, 1],
    strokeWidth: 2.5,
    scaleY: [1, 1.2, 1.1],
    y: [0, 0.5, 0.3],
    transition: {
      duration: 0.6,
      times: [0, 0.5, 1],
      ease: 'easeInOut'
    }
  }
}

const eyeVariants = {
  normal: {
    scale: 1,
    opacity: 1,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scale: [1, 1.3, 1, 1.7],
    opacity: [1, 1, 1, 1],
    transition: {
      duration: 0.6,
      times: [0, 0.3, 0.6, 1],
      ease: 'easeInOut'
    }
  }
}

const startAnimation = () => {
  currentState.value = 'animate'
}

const stopAnimation = () => {
  currentState.value = 'normal'
}

const handleMouseEnter = () => {
  if (!isControlled.value) {
    startAnimation()
  } else {
    emit('startAnimation')
  }
}

const handleMouseLeave = () => {
  if (!isControlled.value) {
    stopAnimation()
  } else {
    emit('stopAnimation')
  }
}

defineExpose({
  startAnimation,
  stopAnimation
})
</script>

<template>
  <div
    :class="[
      'cursor-pointer select-none p-2 hover:bg-accent rounded-md transition-colors duration-200 flex items-center justify-center',
      props.class
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
      <motion.circle
        cx="12"
        cy="12"
        r="10"
        :variants="faceVariants"
        :animate="currentState"
        :initial="'normal'"
      />
      <motion.path
        :variants="mouthVariants"
        :animate="currentState"
        :initial="'normal'"
        d="M18 13a6 6 0 0 1-6 5 6 6 0 0 1-6-5h12Z"
      />
      <motion.line
        x1="9"
        x2="9.01"
        y1="9"
        y2="9"
        :variants="eyeVariants"
        :animate="currentState"
        :initial="'normal'"
      />
      <motion.line
        x1="15"
        x2="15.01"
        y1="9"
        y2="9"
        :variants="eyeVariants"
        :animate="currentState"
        :initial="'normal'"
      />
    </svg>
  </div>
</template> 