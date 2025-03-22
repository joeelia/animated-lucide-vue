<template>
  <UContainer class="py-12">
    <div class="mb-12 flex items-center gap-4 justify-between">
      <div>
        <h1 class="text-2xl font-bold">Lucide Vue Motion Animated Icons</h1>
        <p class="text-sm text-gray-500 mt-4">
          A collection of animated icons using Lucide and Vue Motion. Ported
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
      </div>
      <div>
        <UButton icon="i-mdi-github" color="neutral" variant="ghost" />
      </div>
    </div>
    <div class="grid grid-cols-12 gap-4">
      <AnimatedIconButton>
        <template #default="{ ref, controlled }">
          <LucideAArrowDown
            :ref="ref"
            :controlled="controlled"
            class="text-primary-500"
          />
        </template>
      </AnimatedIconButton>
      <AnimatedIconButton>
        <template #default="{ ref, controlled }">
          <LucideEarth
            :ref="ref"
            :controlled="controlled"
            class="text-primary-500"
          />
        </template>
      </AnimatedIconButton>
      <AnimatedIconButton>
        <template #default="{ ref, controlled }">
          <LucideAudioLines
            :ref="ref"
            :controlled="controlled"
            class="text-primary-500"
          />
        </template>
      </AnimatedIconButton>
      <AnimatedIconButton>
        <template #default="{ ref, controlled }">
          <LucideSquareStack
            :ref="ref"
            :controlled="controlled"
            class="text-primary-500"
          />
        </template>
      </AnimatedIconButton>
      <AnimatedIconButton>
        <template #default="{ ref, controlled }">
          <LucideSlidersHorizontal
            :ref="ref"
            :controlled="controlled"
            class="text-primary-500"
          />
        </template>
      </AnimatedIconButton>
    </div>
  </UContainer>
</template>
