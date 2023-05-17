<script setup lang="ts">
import { computed } from "vue";
import useStore from "@src/store/store";
import FadeTransition from "@src/components/ui/transitions/FadeTransition.vue";
import Conversations from "@src/components/views/HomeView/Sidebar/Conversations/Conversations.vue";

const store = useStore();

// the selected sidebar component
const ActiveComponent = computed(() => {
  if (store.activeSidebarComponent === "messages") {
    return Conversations;
  }
}) as any;
</script>

<template>
  <aside
    class="xs:w-full md:w-[290px] max-h-[90vh] flex flex-col overflow-visible transition-all duration-500 px-5"
  >
    <FadeTransition>
      <component
        :is="ActiveComponent"
        class="h-full flex flex-col max-h-[90vh]"
      />
    </FadeTransition>
  </aside>
</template>
