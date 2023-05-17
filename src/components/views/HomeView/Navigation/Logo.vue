<script setup lang="ts">
import { ref } from "vue";
import useStore from "@src/store/store";
import { ForwardIcon } from "@heroicons/vue/24/outline";

let SkipLinkFocused = ref(false);

const store = useStore();
</script>

<template>
  <div class="flex flex-row">
    <a
      href="#mainContent"
      title="Skip to main content"
      @blur="SkipLinkFocused = false"
      @focus="SkipLinkFocused = true"
      class="outline-none"
      :class="{
        absolute: !SkipLinkFocused,
        'right-[10000px]': !SkipLinkFocused,
      }"
    >
      <ForwardIcon class="w-6 h-5 text-indigo-300" />
    </a>

    <button
      aria-label="heycenter logo"
      :class="{ hidden: SkipLinkFocused }"
      class="outline-none p-2 w-[48px] h-[48px] justify-center items-center rounded-2xl flex flex-row gap-2"
    >
      <img
        v-if="!store.settings.darkMode"
        src="@src/assets//vectors/hey_logo.svg"
        class="w-8 h-8"
        alt="logo"
      />
      <img
        v-else
        src="@src/assets/vectors/hey_logo.svg"
        class="w-8 h-8 opacity-40"
        alt="white logo"
      />
      <p class="text-white font-bold text-sm">hey!</p>
    </button>
  </div>
</template>
