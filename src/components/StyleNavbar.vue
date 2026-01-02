<template>
  <nav
    class="mx-auto max-w-6xl mb-6 flex flex-col md:flex-row items-center justify-between gap-4 p-4 rounded-xl"
    :class="specialStyles.includes(modelValue) ? activeStyle.card : 'bg-white/10 backdrop-blur-md border border-white/20'"
  >
    <div class="flex items-center gap-3">
      <div
        class="p-2 rounded-lg"
        :class="activeStyle.highlight.includes('text-white') || activeStyle.bg.includes('bg-black')
          ? 'bg-white text-black'
          : 'bg-current text-white'"
      >
        <LucideGamepad2 class="w-6 h-6" />
      </div>
      <h1 class="text-2xl font-bold" :class="activeStyle.text">
        UI Style Museum
      </h1>
    </div>

    <div class="flex items-center gap-4">
      <label class="text-sm font-medium hidden md:block" :class="activeStyle.text">
        Select Style:
      </label>
      <div class="relative">
        <select
          v-model="modelValue"
          class="appearance-none cursor-pointer pl-4 pr-10 py-2 w-64 text-sm outline-none transition-all bg-black text-white border border-gray-700 rounded-md focus:ring-2 focus:ring-white"
        >
          <optgroup label="Classic / Modern" class="bg-black text-white">
            <option
              v-for="key in classicModern"
              :key="key"
              :value="key"
              class="bg-black text-white"
            >
              {{ styles[key].name }}
            </option>
          </optgroup>
          <optgroup label="Enterprise / Technical" class="bg-black text-white">
            <option
              v-for="key in enterpriseTech"
              :key="key"
              :value="key"
              class="bg-black text-white"
            >
              {{ styles[key].name }}
            </option>
          </optgroup>
          <optgroup label="Retro / Brutalist" class="bg-black text-white">
            <option
              v-for="key in retroBrutalist"
              :key="key"
              :value="key"
              class="bg-black text-white"
            >
              {{ styles[key].name }}
            </option>
          </optgroup>
          <optgroup label="Game / Movie (NEW)" class="bg-black text-white">
            <option
              v-for="key in gameMovie"
              :key="key"
              :value="key"
              class="bg-black text-white"
            >
              {{ styles[key].name }}
            </option>
          </optgroup>
        </select>
        <div
          class="absolute right-3 top-1/2 -translate-y-1/2 pointer-events-none opacity-50"
          :class="activeStyle.text"
        >
          <LucideArrowRight class="w-4 h-4 rotate-90" />
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup lang="ts">
import type { StyleConfig, StyleKey } from '@/utils/styles'
import { styles } from '@/utils/styles'

defineProps<{
  activeStyle: StyleConfig
}>()

const modelValue = defineModel<StyleKey>({ required: true })

// 風格分組
const classicModern: StyleKey[] = ['arc', 'tremor', 'shadcn', 'cupertino', 'material3', 'notion', 'vercel', 'linear']
const enterpriseTech: StyleKey[] = ['supabase', 'grafana', 'salesforce', 'carbon', 'ant', 'metronic', 'adminlte']
const retroBrutalist: StyleKey[] = ['neobrutalism', 'win95']
const gameMovie: StyleKey[] = ['matrix', 'acnh', 'mario', 'cyberpunk', 'minecraft', 'fallout', 'persona', 'gta', 'pokemon', 'elden']

// 需要特殊卡片樣式的風格
const specialStyles: StyleKey[] = [
  'neobrutalism', 'cyberpunk', 'persona', 'minecraft', 'fallout', 'gta', 'pokemon', 'win95', 'mario', 'elden', 'matrix', 'adminlte', 'carbon'
]
</script>
