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
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scale: [1, 1.05, 0.98, 1.02],
    rotate: [0, 1, -1, 0],
    transition: {
      duration: 0.7,
      times: [0, 0.4, 0.7, 1],
      ease: 'easeInOut'
    }
  }
}

const mouthVariants = {
  normal: {
    scaleX: 1,
    y: 0,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scaleX: [1, 1.2, 0.9, 1.1],
    y: [0, 0.5, -0.5, 0],
    transition: {
      duration: 0.6,
      times: [0, 0.3, 0.6, 1],
      ease: 'easeInOut',
      delay: 0.1
    }
  }
}

const leftEyeVariants = {
  normal: {
    scale: 1,
    x: 0,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scale: [1, 1.3, 1, 1.2],
    x: [0, -0.3, 0.3, 0],
    transition: {
      duration: 0.5,
      times: [0, 0.3, 0.6, 1],
      ease: 'easeInOut'
    }
  }
}

const rightEyeVariants = {
  normal: {
    scale: 1,
    x: 0,
    transition: { duration: 0.3, ease: 'easeOut' }
  },
  animate: {
    scale: [1, 1.3, 1, 1.2],
    x: [0, 0.3, -0.3, 0],
    transition: {
      duration: 0.5,
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
      <motion.line
        :variants="mouthVariants"
        :animate="currentState"
        :initial="'normal'"
        x1="8"
        x2="16"
        y1="15"
        y2="15"
      />
      <motion.line
        x1="9"
        x2="9.01"
        y1="9"
        y2="9"
        :variants="leftEyeVariants"
        :animate="currentState"
        :initial="'normal'"
      />
      <motion.line
        x1="15"
        x2="15.01"
        y1="9"
        y2="9"
        :variants="rightEyeVariants"
        :animate="currentState"
        :initial="'normal'"
      />
    </svg>
  </div>
</template> 