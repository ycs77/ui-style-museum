<template>
  <div
    class="min-h-screen transition-colors duration-500 ease-in-out p-4 md:p-8"
    :class="[activeStyle.bg, activeStyle.font]"
  >
    <!-- 頂部導航欄 -->
    <StyleNavbar v-model="currentStyleKey" :active-style />

    <!-- AI Prompt Section -->
    <PromptCard
      :prompt="activeStyle.prompt"
      :active-style
      :current-style-key
    />

    <div class="max-w-6xl mx-auto grid grid-cols-1 lg:grid-cols-12 gap-8">
      <!-- 左側：資訊面板 -->
      <div class="lg:col-span-4 space-y-6">
        <!-- Style Info -->
        <div class="p-6 transition-all duration-300" :class="activeStyle.card">
          <h2 class="text-xl font-bold mb-2" :class="activeStyle.text">Style Info</h2>
          <div
            class="h-1 w-12 mb-4 rounded-full"
            :class="[activeStyle.highlight, activeStyle.highlight.includes('bg-') ? '' : 'bg-current']"
          />

          <div class="space-y-4">
            <div>
              <span
                class="text-xs uppercase tracking-wider opacity-60 font-semibold"
                :class="activeStyle.text"
              >
                Name
              </span>
              <p class="text-lg font-medium" :class="activeStyle.text">
                {{ activeStyle.name }}
              </p>
            </div>

            <div>
              <span
                class="text-xs uppercase tracking-wider opacity-60 font-semibold"
                :class="activeStyle.text"
              >
                Characteristics
              </span>
              <p class="text-sm opacity-80" :class="activeStyle.text">
                {{ activeStyle.desc }}
              </p>
            </div>

            <div>
              <span
                class="text-xs uppercase tracking-wider opacity-60 font-semibold"
                :class="activeStyle.text"
              >
                Prompt Keywords
              </span>
              <div class="mt-2 flex flex-wrap gap-2">
                <span
                  v-for="(keyword, i) in keywords"
                  :key="i"
                  class="px-2 py-1 text-xs"
                  :class="activeStyle.badge"
                >
                  {{ keyword }}
                </span>
              </div>
            </div>
          </div>
        </div>

        <!-- Elements Preview -->
        <div class="p-6 transition-all duration-300" :class="activeStyle.card">
          <h3
            class="text-sm font-bold uppercase tracking-wider mb-4 opacity-70"
            :class="activeStyle.text"
          >
            Elements
          </h3>
          <div class="space-y-4">
            <button
              class="w-full py-2 px-4 flex items-center justify-center gap-2"
              :class="activeStyle.button"
            >
              <LucideMousePointer2 class="w-4 h-4" />
              Primary Action
            </button>
            <button class="w-full py-2 px-4" :class="activeStyle.buttonSec">
              Secondary Action
            </button>
            <div class="flex gap-2">
              <span class="px-3 py-1 text-xs" :class="activeStyle.badge">Active</span>
              <span class="px-3 py-1 text-xs opacity-50" :class="activeStyle.badge">
                Inactive
              </span>
            </div>
          </div>
        </div>
      </div>

      <!-- 右側：互動預覽區 -->
      <div class="lg:col-span-8 space-y-6">
        <!-- 統計卡片 -->
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
          <div
            v-for="(stat, idx) in stats"
            :key="idx"
            class="p-5 transition-all duration-300 flex flex-col justify-between"
            :class="activeStyle.card"
          >
            <div class="flex justify-between items-start mb-2">
              <span class="text-sm opacity-70 font-medium" :class="activeStyle.text">
                {{ stat.label }}
              </span>
              <component :is="stat.icon" class="w-4 h-4 opacity-50" :class="activeStyle.text" />
            </div>
            <div>
              <div class="text-2xl font-bold" :class="activeStyle.text">
                {{ stat.val }}
              </div>
              <p
                class="text-xs mt-1"
                :class="activeStyle.highlight.includes('text-white') ? 'text-green-400' : 'text-green-600'"
              >
                {{ stat.change }}
                <span class="opacity-60" :class="activeStyle.text">from last month</span>
              </p>
            </div>
          </div>
        </div>

        <!-- Project Dashboard -->
        <div class="p-6 md:p-8 transition-all duration-300" :class="activeStyle.card">
          <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4">
            <div>
              <h2 class="text-2xl font-bold" :class="activeStyle.text">
                Project Dashboard
              </h2>
              <p class="text-sm opacity-60" :class="activeStyle.text">
                Manage your team and tasks efficiently.
              </p>
            </div>
            <button class="px-4 py-2 flex items-center gap-2" :class="activeStyle.button">
              <LucideSettings class="w-4 h-4" />
              Settings
            </button>
          </div>

          <div class="space-y-6">
            <!-- 搜尋與篩選 -->
            <div class="flex gap-3">
              <div class="relative flex-1">
                <LucideSearch
                  class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 opacity-50"
                  :class="activeStyle.text"
                />
                <input
                  type="text"
                  placeholder="Search tasks..."
                  class="w-full pl-10 pr-4 py-2 outline-none transition-all"
                  :class="activeStyle.input"
                />
              </div>
              <button class="px-4 py-2 hidden sm:block" :class="activeStyle.buttonSec">
                Filter
              </button>
            </div>

            <!-- 任務列表 -->
            <div class="space-y-3">
              <div
                v-for="(task, i) in tasks"
                :key="i"
                class="group flex items-center justify-between p-3 md:p-4 rounded-lg transition-all border"
                :class="getTaskBorderClass(currentStyleKey)"
              >
                <div class="flex items-center gap-4">
                  <div
                    class="w-5 h-5 rounded-full border flex items-center justify-center cursor-pointer"
                    :class="i === 2
                      ? activeStyle.highlight.includes('text-white')
                        ? 'bg-green-500 border-green-500'
                        : 'bg-green-500 border-green-500'
                      : 'border-gray-400 opacity-50'"
                  >
                    <LucideCheckCircle2 v-if="i === 2" class="w-3 h-3 text-white" />
                  </div>
                  <div>
                    <h4
                      class="font-medium text-sm"
                      :class="[i === 2 ? 'line-through opacity-50' : '', activeStyle.text]"
                    >
                      {{ task.title }}
                    </h4>
                    <span class="text-xs opacity-60" :class="activeStyle.text">
                      {{ task.tag }}
                    </span>
                  </div>
                </div>
                <span
                  class="text-xs px-2 py-1 rounded whitespace-nowrap"
                  :class="task.status === 'Done'
                    ? 'bg-green-100 text-green-700'
                    : task.status === 'Pending'
                    ? 'bg-yellow-100 text-yellow-700'
                    : activeStyle.badge"
                >
                  {{ task.status }}
                </span>
              </div>
            </div>

            <!-- 底部行動區 -->
            <div
              class="pt-4 border-t flex justify-between items-center"
              :class="activeStyle.text.includes('text-white') ? 'border-white/10' : 'border-black/5'"
            >
              <p class="text-sm opacity-50" :class="activeStyle.text">
                Showing 4 of 12 tasks
              </p>
              <div class="flex gap-2">
                <button
                  class="px-3 py-1 text-sm disabled:opacity-50"
                  :class="activeStyle.buttonSec"
                  disabled
                >
                  Prev
                </button>
                <button class="px-3 py-1 text-sm" :class="activeStyle.buttonSec">
                  Next
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { StyleConfig, StyleKey } from '@/types/styles'
import { styles } from '@/types/styles'

// State
const currentStyleKey = ref<StyleKey>('shadcn')

// Computed
const activeStyle = computed<StyleConfig>(() => styles[currentStyleKey.value])
const keywords = computed(() => activeStyle.value.keywords.split(', '))

// 統計卡片資料
const stats = [
  { label: 'Total Revenue', val: '$45,231.89', change: '+20.1%', icon: 'LucideBarChart3' },
  { label: 'Active Users', val: '+2350', change: '+180.1%', icon: 'LucideUser' },
  { label: 'Sales', val: '+12,234', change: '+19%', icon: 'LucideActivity' }
]

// 任務列表資料
const tasks = [
  { title: 'Redesign Homepage', tag: 'Design', status: 'In Progress' },
  { title: 'Fix API Latency', tag: 'Backend', status: 'Pending' },
  { title: 'Update Documentation', tag: 'Docs', status: 'Done' },
  { title: 'Mobile Responsive Check', tag: 'QA', status: 'In Progress' }
]

// 任務列表邊框樣式
function getTaskBorderClass(styleKey: StyleKey): string {
  const specialBorderStyles = ['neobrutalism', 'minecraft', 'pokemon', 'win95']
  const transparentBorderStyles = ['cyberpunk', 'matrix', 'carbon']

  if (specialBorderStyles.includes(styleKey)) {
    return 'border-2 border-black'
  }
  if (transparentBorderStyles.includes(styleKey)) {
    return 'border border-current bg-transparent'
  }
  return 'border-transparent hover:bg-black/5 dark:hover:bg-white/5'
}
</script>
