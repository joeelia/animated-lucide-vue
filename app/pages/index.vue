<template>
  <UContainer class="py-12">
    <div class="mb-12 flex items-start md:items-center gap-4 justify-between">
      <div>
        <h1 class="text-2xl font-bold">Animated Lucide Vue Icons</h1>
        <p class="text-sm text-gray-500 mt-4">
          A collection of {{ totalIcons }} animated icons using Lucide and Vue Motion. Ported
          from the awesome lib
          <a
            href="https://icons.pqoqubbw.dev/"
            class="text-primary-500 hover:underline"
            >pqoqubbw/icons</a
          >
          by
          <a
            href="https://x.com/pqoqubbw"
            class="text-primary-500 hover:underline"
            >dmytro</a
          >
        </p>
        <p class="text-sm text-gray-500 mt-4">
          Made by
          <a
            href="https://x.com/fayazara"
            class="text-primary-500 hover:underline"
            >Fayaz</a
          >. and
          <a
            href="https://x.com/Joe_Elia"
            class="text-primary-500 hover:underline"
            >Joe Elia</a
          >. Work in progress, feel free to contribute on GitHub.
        </p>
      </div>
      <div class="flex items-center gap-2">
        <ThemeToggle/>
        <ThemePicker v-model="primaryColor" />
        <UButton
          icon="i-lucide-github"
          variant="ghost"
          target="_blank"
          to="https://github.com/fayazara/animated-lucide-vue"
        />
      </div>
    </div>

    <div class="mb-8">
      <input
        v-model="searchQuery"
        type="text"
        :placeholder="`Search ${totalIcons} icons...`"
        class="w-full px-4 py-2 rounded-lg border dark:border-gray-700 bg-white dark:bg-gray-800 focus:outline-none focus:ring-2 focus:ring-primary-500"
      />
      <div v-if="searchQuery" class="mt-2 text-sm text-gray-500">
        Results: {{ filteredIcons.length }}
      </div>
    </div>

    <div class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-10 gap-4">
      <div v-for="icon in filteredIcons" :key="icon" class="flex flex-col items-center gap-1">
        <AnimatedIconButton class="w-14 h-14 bg-gray-100 dark:bg-gray-800 rounded-lg">
        <template #default="{ ref, controlled }">
            <component
              :is="getIconComponent(icon)"
            :ref="ref"
            :controlled="controlled"
            :class="primaryColor"
              :size="24"
          />
        </template>
      </AnimatedIconButton>
        <span class="text-xs text-gray-500 select-text cursor-text text-center w-full truncate">{{ icon }}</span>
      </div>
    </div>
  </UContainer>
</template>

<script setup lang="ts">
import { ref, computed, defineAsyncComponent } from 'vue'

const primaryColor = ref('text-neutral-700 dark:text-neutral-300')
const searchQuery = ref('')

const icons: string[] = [
  'scan-face',
  'smartphone-charging',
  'history',
  'square-activity',
  'square-arrow-down',
  'square-arrow-left',
  'square-arrow-right',
  'square-arrow-up',
  'key',
  'key-square',
  'key-circle',
  'rotate-cw',
  'rotate-ccw',
  'refresh-cw-off',
  'refresh-cw',
  'refresh-ccw-dot',
  'redo',
  'undo-dot',
  'redo-dot',
  'arrow-big-down',
  'arrow-big-left',
  'arrow-big-right',
  'arrow-big-up',
  'a-arrow-up',
  'chart-spline',
  'arrow-up',
  'arrow-down',
  'vibrate',
  'waves-ladder',
  'waves',
  'wind-arrow-down',
  'air-vent',
  'tornado',
  'cloud-rain',
  'cloud-rain-wind',
  'arrow-big-down-dash',
  'arrow-big-left-dash',
  'arrow-big-right-dash',
  'arrow-big-up-dash',
  'moon',
  'facebook',
  'twitter',
  'linkedin',
  'youtube',
  'instagram',
  'twitch',
  'dribbble',
  'discord',
  'search',
  'cloud-sun',
  'sunset',
  'sun-dim',
  'sun-medium',
  'sun-moon',
  'cart',
  'stethoscope',
  'circle-check',
  'earth',
  'workflow',
  'logout',
  'circle-help',
  'menu',
  'moon-alt',
  'settings',
  'sliders-horizontal',
  'square-stack',
  'sun',
  'user-round',
  'users',
  'alarm-clock',
  'audio-lines',
  'bell',
  'bluetooth-searching',
  'calendar-days',
  'connect',
  'delete',
  'file-stack',
  'home',
  'frown',
  'smile-plus',
  'smile',
  'laugh',
  'annoyed',
  'meh'
]

const filteredIcons = computed(() => {
  if (!searchQuery.value) return icons
  return icons.filter(icon => 
    icon.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const totalIcons = computed(() => icons.length)

const getIconComponent = computed(() => (icon: string) => {
  const componentName = icon.split('-').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join('')
  return defineAsyncComponent(() => import(`~/components/Lucide/${componentName}.vue`))
})
</script>
