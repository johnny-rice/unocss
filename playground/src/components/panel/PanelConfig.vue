<script lang='ts' setup>
import { Pane } from 'splitpanes'
import { customConfigRaw } from '../../composables/url'

defineProps<{ index: number }>()

if (!customConfigRaw.value)
  customConfigRaw.value = defaultConfigRaw
</script>

<template>
  <Pane :min-size="titleHeightPercent" :size="panelSizes[index]" flex flex-col relative>
    <TitleBar
      title="Config"
      @title-click="togglePanel(index)"
    >
      <template #before>
        <div
          class="flex-shrink-0 i-ri-arrow-right-s-line transition-transform transform"
          :class="isCollapsed(index) ? '' : 'rotate-90'"
        />
      </template>
      <div
        flex flex-1 justify-end items-center w-full h-full gap2
        transition duration-400
        :class="isCollapsed(index) ? 'op0' : ''"
        un-children="inline-flex items-center cursor-pointer gap1"
      >
        <div w-1px h-full bg-gray:20 />
        <button
          i-ri-mist-line icon-btn
          title="Format"
          @click="formatConfig()"
        />
      </div>
    </TitleBar>
    <CodeMirror
      v-model="customConfigRaw"
      flex-auto
      mode="js"
      border="l main"
      class="scrolls"
    />
    <div
      v-if="!isCollapsed(index) && customConfigError"
      absolute
      left-0
      right-0
      bottom-0
      p="x-2 y-1"
      bg="red-400/20"
      text="red-400 sm"
    >
      {{ customConfigError.toString() }}
    </div>
  </Pane>
</template>
