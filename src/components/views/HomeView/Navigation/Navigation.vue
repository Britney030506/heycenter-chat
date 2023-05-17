<script setup lang="ts">
import { ref } from "vue";

import useStore from "@src/store/store";

import {
  Cog6ToothIcon,
  MoonIcon,
  SunIcon,
  UserIcon,
} from "@heroicons/vue/24/solid";

import Logo from "@src/components/views/HomeView/Navigation/Logo.vue";
import NavLink from "@src/components/views/HomeView/Navigation/NavLink.vue";
import Typography from "@src/components/ui/data-display/Typography.vue";

const store = useStore();

const showDropdown = ref(false);

// (event) change the active sidebar component when clicking on a NavLink
const handleActiveSidebarComponentChange = (value: string) => {
  store.activeSidebarComponent = value;
};
</script>

<template>
  <div
    class="flex flex-row h-[10vh] w-full transition-all duration-500 items-center justify-between px-8 bg-[#5A74FD]"
  >
    <!--logo-->
    <div @click="() => handleActiveSidebarComponentChange('messages')">
      <Logo />
    </div>
    <!--main navigation-->

    <!--title-->
    <p class="text-white font-bold text-sm">HEYCENTER</p>

    <!--secondary navigation-->
    <div>
      <nav aria-label="Extra navigation" class="block">
        <ul class="flex flex-row items-center gap-[14px] justify-between">
          <!--toggle dark mode button-->
          <li>
            <NavLink
              :icon="store.settings.darkMode ? SunIcon : MoonIcon"
              title="Night mode"
              @click="store.settings.darkMode = !store.settings.darkMode"
            />
          </li>
          <!-- <AccountDropdown
            id="profile-menu"
            class="xs:hidden md:block"
            aria-labelledby="profile-menu-button"
            :show-dropdown="showDropdown"
            :handle-show-dropdown="() => {}"
            :handle-close-dropdown="() => {}"
          /> -->
          <li>
            <NavLink :icon="UserIcon" title="Profile" />
          </li>
          <!--settings button-->
          <li>
            <NavLink
              :icon="Cog6ToothIcon"
              title="Settings"
              :active="store.activeSidebarComponent === 'settings'"
            />
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>
