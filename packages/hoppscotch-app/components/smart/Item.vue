<template>
  <SmartLink
    :to="`${/^\/(?!\/).*$/.test(to) ? localePath(to) : to}`"
    :exact="exact"
    :blank="blank"
    class="hover:bg-primaryDark hover:text-secondaryDark focus:outline-none focus-visible:bg-primaryDark focus-visible:text-secondaryDark inline-flex items-center flex-shrink-0 px-4 py-2 transition rounded"
    :class="[
      { 'opacity-75 cursor-not-allowed': disabled },
      { 'pointer-events-none': loading },
      { 'flex-1': label },
      { 'flex-row-reverse justify-end': reverse },
      {
        'border border-divider hover:border-dividerDark focus-visible:border-dividerDark':
          outline,
      },
    ]"
    :disabled="disabled"
    :tabindex="loading ? '-1' : '0'"
  >
    <span
      v-if="!loading"
      class="inline-flex items-center"
      :class="{ 'self-start': infoIcon }"
    >
      <i
        v-if="icon"
        :class="[
          label ? (reverse ? 'ml-4 opacity-75' : 'mr-4 opacity-75') : '',
          { 'text-accent': active },
        ]"
        class="material-icons"
      >
        {{ icon }}
      </i>
      <SmartIcon
        v-if="svg"
        :name="svg"
        :class="[
          label ? (reverse ? 'ml-4 opacity-75' : 'mr-4 opacity-75') : '',
          { 'text-accent': active },
        ]"
        class="svg-icons"
      />
    </span>
    <SmartSpinner v-else class="text-secondaryDark mr-4" />
    <div
      class="inline-flex items-start flex-1 truncate"
      :class="{ 'flex-col': description }"
    >
      <div class="font-semibold truncate">
        {{ label }}
      </div>
      <p
        v-if="description"
        class="text-secondaryLight my-2 font-medium text-left"
      >
        {{ description }}
      </p>
    </div>
    <i
      v-if="infoIcon"
      class="material-icons items-center self-center ml-6"
      :class="{ 'text-accent': activeInfoIcon }"
    >
      {{ infoIcon }}
    </i>
  </SmartLink>
</template>

<script>
import { defineComponent } from "@nuxtjs/composition-api"

export default defineComponent({
  props: {
    to: {
      type: String,
      default: "",
    },
    exact: {
      type: Boolean,
      default: true,
    },
    blank: {
      type: Boolean,
      default: false,
    },
    label: {
      type: String,
      default: "",
    },
    description: {
      type: String,
      default: "",
    },
    icon: {
      type: String,
      default: "",
    },
    svg: {
      type: String,
      default: "",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
    reverse: {
      type: Boolean,
      default: false,
    },
    outline: {
      type: Boolean,
      default: false,
    },
    active: {
      type: Boolean,
      default: false,
    },
    activeInfoIcon: {
      type: Boolean,
      default: false,
    },
    infoIcon: {
      type: String,
      default: "",
    },
  },
})
</script>
