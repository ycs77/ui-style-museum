<template>
  <div class="max-w-6xl mx-auto mb-8">
    <div
      class="p-4 rounded-xl border flex flex-col md:flex-row gap-4 items-start md:items-center justify-between"
      :class="specialStyles.includes(currentStyleKey) ? activeStyle.card : 'bg-white/5 backdrop-blur-sm border-white/10'"
    >
      <div class="flex-1">
        <div class="flex items-center gap-2 mb-1">
          <LucideSparkles
            class="w-4 h-4"
            :class="activeStyle.highlight.includes('text-white') ? 'text-white' : activeStyle.highlight"
          />
          <span
            class="text-xs font-bold uppercase tracking-wider opacity-70"
            :class="activeStyle.text"
          >
            AI Prompt / 詠唱文
          </span>
        </div>
        <p class="text-sm font-mono opacity-90" :class="activeStyle.text">
          {{ prompt }}
        </p>
      </div>
      <button
        @click="copy(prompt)"
        class="px-4 py-2 flex items-center gap-2 transition-all shrink-0"
        :class="copied ? 'bg-green-500 text-white border-green-500' : activeStyle.button"
      >
        <LucideCheck v-if="copied" class="w-4 h-4" />
        <LucideCopy v-else class="w-4 h-4" />
        {{ copied ? 'Copied!' : 'Copy Prompt' }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { StyleConfig, StyleKey } from '@/utils/styles'
import { useClipboard } from '@vueuse/core'

defineProps<{
  prompt: string
  activeStyle: StyleConfig
  currentStyleKey: StyleKey
}>()

const { copy, copied } = useClipboard()

// 需要特殊卡片樣式的風格
const specialStyles: StyleKey[] = [
  'neobrutalism', 'cyberpunk', 'persona', 'minecraft', 'gta', 'win95'
]
</script>
