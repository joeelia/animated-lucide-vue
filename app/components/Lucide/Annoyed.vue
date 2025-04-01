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
    transition: { duration: 0.2, ease: 'easeOut' }
  },
  animate: {
    scale: 1.05,
    transition: {
      duration: 0.3,
      ease: 'easeOut'
    }
  }
}

const mouthVariants = {
  normal: {
    scaleX: 1,
    y: 0,
    transition: { duration: 0.2, ease: 'easeOut' }
  },
  animate: {
    scaleX: 0.8,
    y: 1,
    transition: {
      duration: 0.3,
      ease: 'easeOut'
    }
  }
}

const leftEyebrowVariants = {
  normal: {
    rotate: 0,
    y: 0,
    x: 0,
    transition: { duration: 0.2, ease: 'easeOut' }
  },
  animate: {
    rotate: 15,
    y: -1,
    x: -0.5,
    transition: {
      duration: 0.25,
      ease: 'easeOut'
    }
  }
}

const rightEyebrowVariants = {
  normal: {
    rotate: 0,
    y: 0,
    x: 0,
    transition: { duration: 0.2, ease: 'easeOut' }
  },
  animate: {
    rotate: 15,
    y: -1,
    x: 0.5,
    transition: {
      duration: 0.25,
      ease: 'easeOut',
      delay: 0.05
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
        d="M8 15h8"
        :variants="mouthVariants"
        :animate="currentState"
        :initial="'normal'"
      />
      <motion.path
        d="M8 9h2"
        :variants="leftEyebrowVariants"
        :animate="currentState"
        :initial="'normal'"
      />
      <motion.path
        d="M14 9h2"
        :variants="rightEyebrowVariants"
        :animate="currentState"
        :initial="'normal'"
      />
    </svg>
  </div>
</template> 