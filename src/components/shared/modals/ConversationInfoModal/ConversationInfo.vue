<script setup lang="ts">
import { ArrowUturnLeftIcon } from "@heroicons/vue/24/solid";
import { computed, ref } from "vue";
import { IContact, IConversation } from "@src/types";
import { getAvatar, getFullName, getName, getOddContact } from "@src/utils";
import {
  ArrowsRightLeftIcon,
  BellIcon,
  CheckCircleIcon,
  NoSymbolIcon,
  PencilIcon,
  PhoneIcon,
  ShareIcon,
  TrashIcon,
  UserIcon,
  XCircleIcon,
} from "@heroicons/vue/24/outline";
import InfoItem from "@src/components/shared/blocks/InfoItem.vue";
import ImageViewer from "@src/components/shared/modals/ConversationInfoModal/ImageViewer.vue";
import Typography from "@src/components/ui/data-display/Typography.vue";
import Button from "@src/components/ui/inputs/Button.vue";
import IconButton from "@src/components/ui/inputs/IconButton.vue";

const props = defineProps<{
  conversation: IConversation;
  contact?: IContact;
  closeModal: () => void;
}>();

const openImageViewer = ref(false);

const imageUrl = computed(() => {
  if (props.contact) {
    return props.contact.avatar;
  } else {
    return getAvatar(props.conversation);
  }
});
</script>

<template>
  <div class="">
    <div class="mb-6 px-4 flex justify-between items-center">
      <!--title-->
      <Typography
        variant="heading-1"
        id="modal-title"
        class="default-outline"
        tabindex="0"
      >
        <span v-if="conversation.type === 'couple' || props.contact">
          Contacto
        </span>
      </Typography>

      <!--close button-->
      <Button
        v-if="!props.contact"
        @click="props.closeModal"
        variant="outlined"
        color="danger"
        typography="body-4"
      >
        X
      </Button>

      <!--return button-->
      <button
        v-else
        @click="
          $emit('active-page-change', {
            tabName: 'members',
            animationName: 'slide-right',
          })
        "
        class="group p-2 border rounded-full border-gray-200 dark:border-white dark:border-opacity-70 focus:outline-none focus:border-indigo-100 focus:bg-indigo-100 hover:bg-indigo-100 hover:border-indigo-100 dark:hover:border-indigo-400 dark:hover:bg-indigo-400 dark:focus:bg-reindigod-400 dark:focus:border-indigo-400 transition-all duration-200 outline-none"
      >
        <ArrowUturnLeftIcon
          class="w-5 h-5 text-black opacity-100 dark:text-white dark:opacity-70 group-hover:text-indigo-500 group-hover:opacity-100 dark:group-hover:text-white"
        />
      </button>
    </div>

    <!--top-->
    <div class="w-full p-5 pb-6">
      <div class="flex flex-col justify-center items-center">
        <!--avatar-->
        <div class="mb-5">
          <button
            v-if="props.contact"
            @click="openImageViewer = true"
            class="outline-none"
            aria-label="view avatar"
          >
            <div
              :style="{ backgroundImage: `url(${props.contact.avatar})` }"
              class="w-[62px] h-[62px] rounded-full bg-cover bg-center"
            ></div>
          </button>

          <button
            v-else
            @click="openImageViewer = true"
            class="outline-none"
            aria-label="view avatar"
          >
            <div
              :style="{
                backgroundImage: `url(${getAvatar(props.conversation)})`,
              }"
              class="w-[150px] h-[150px] rounded-full bg-cover bg-center"
            ></div>
          </button>
        </div>

        <!--name-->
        <div class="w-full flex justify-center items-center">
          <div>
            <Typography variant="heading-2" class="text-center">
              <span v-if="props.contact">
                {{ getFullName(props.contact) }}
              </span>

              <span v-else>
                {{ getName(props.conversation) }}
              </span>
            </Typography>
          </div>

          <IconButton
            title="edit group"
            v-if="['group', 'broadcast'].includes(conversation.type)"
            class="group w-7 h-7"
          >
            <PencilIcon
              class="w-5 h-5 text-gray-400 group-hover:text-indigo-300"
              @click="
                $emit('active-page-change', {
                  tabName: 'edit-group',
                  animationName: 'slide-left',
                })
              "
            />
          </IconButton>
        </div>
      </div>
    </div>
    <!-- Marcar status -->
    <!--Status Bueno-->
    <div class="p-4 mb-2 border-t border-gray-100">
      <InfoItem title="Marcar estatus de conversacion como:" />
    </div>
    <div
      class="w-full dark:border-gray-700 flex flex-row items-center justify-center px-4 pb-4"
    >
      <!--Status Bueno-->
      <InfoItem :icon="CheckCircleIcon" title="Bueno" link />

      <!--Status malo-->
      <InfoItem :icon="XCircleIcon" title="Malo" link color="danger" />

      <!--Status transfer-->
      <InfoItem :icon="ArrowsRightLeftIcon" title="Cambio" link />
    </div>

    <!--middle-->
    <div class="w-full py-5 border-t border-gray-100 dark:border-gray-700">
      <!--(contact) phone-->
      <div
        v-if="conversation.type === 'couple' || props.contact"
        class="flex px-5 pb-5 items-center"
      >
        <InfoItem :icon="PhoneIcon" :title="56154118791" />
      </div>

      <!--(group) members-->
      <div
        v-if="
          ['group', 'broadcast'].includes(conversation.type) && !props.contact
        "
        class="px-5 flex items-center pb-5"
      >
        <InfoItem
          :icon="UserIcon"
          title="members"
          link
          chevron
          @click="
            $emit('active-page-change', {
              tabName: 'members',
              animationName: 'slide-left',
            })
          "
        />
      </div>

      <!--(both) notifications-->
      <div class="px-5 flex items-center">
        <InfoItem :icon="BellIcon" title="Notificaciones" switch />
      </div>
    </div>

    <!--bottom-->
    <div class="w-full border-t border-gray-100 dark:border-gray-700">
      <!--media-->
      <div class="px-5 pt-5 flex items-center">
        <InfoItem
          :icon="ShareIcon"
          title="Multimedia"
          link
          chevron
          @click="
            $emit('active-page-change', {
              tabName: 'shared-media',
              animationName: 'slide-left',
            })
          "
        />
      </div>
      <!--block contact-->
      <div
        v-if="conversation.type === 'couple' || props.contact"
        class="px-5 pt-5 group"
      >
        <InfoItem
          :icon="NoSymbolIcon"
          title="Bloquear contacto"
          link
          color="danger"
        />
      </div>

      <!--delete contact-->
      <div
        v-if="conversation.type === 'couple' || props.contact"
        class="px-5 pt-5 group"
      >
        <InfoItem
          :icon="TrashIcon"
          title="Eliminar contacto"
          link
          color="danger"
        />
      </div>
    </div>

    <!--image viewer-->
    <ImageViewer
      :image-url="imageUrl"
      :open="openImageViewer"
      :close-image="() => (openImageViewer = false)"
    />
  </div>
</template>
