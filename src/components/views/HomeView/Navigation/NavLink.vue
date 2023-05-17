<script setup lang="ts">
import type { Component } from "vue";

const props = defineProps<{
  icon: Component;
  title: string;
  notifications?: number;
  active?: boolean;
}>();
</script>

<template>
  <div class="xs:mb-0 md:mb-0">
    <button
      class="group relative focus:outline-none"
      :title="props.title"
      :aria-label="
        props.notifications
          ? props.title + ' ' + props.notifications + ' new notifications'
          : props.title
      "
    >
      <!--icon-->
      <component
        :is="props.icon"
        class="w-6 h-6 group-focus:text-[#ffffff] hover:text-[#ffffff] active:text-[#ffffff] active:scale-110 dark:text-[#95A4F6] transition ease-out duration-200"
        :class="(props.active as boolean) ? ['text-[#ffffff]', 'dark:text-[#ffffff]'] : [' text-[#95A4F6]']"
      />

      <!--notification pill-->
      <div
        v-if="props.notifications"
        class="absolute right-0 top-3 w-5 h-5 flex items-center justify-center outline-none bg-[#ffffff] text-xs text-white border border-white dark:border-gray-800 rounded-full transition duration-500"
      >
        {{ props.notifications }}
      </div>
    </button>
  </div>
</template>
