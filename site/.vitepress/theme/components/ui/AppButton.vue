<script setup>
import { computed } from 'vue';

const props = defineProps({
  href: {
    type: String,
    default: '#',
  },
  label: {
    type: String,
    required: true,
  },
  ariaLabel: {
    type: String,
    default: '',
  },
  variant: {
    type: String,
    default: 'windows',
  },
});

const isUnavailable = computed(() => props.href === '#');

function handleClick(event) {
  if (isUnavailable.value) {
    event.preventDefault();
  }
}
</script>

<template>
  <a
    class="app-button"
    :class="`app-button--${variant}`"
    :href="href"
    :aria-label="ariaLabel || label"
    :aria-disabled="String(isUnavailable)"
    @click="handleClick"
  >
    <span class="app-button__fill" aria-hidden="true" />
    <span class="app-button__content">
      <span class="app-button__icon" aria-hidden="true">
        <slot name="icon" />
      </span>
      <span class="app-button__label">{{ label }}</span>
    </span>
  </a>
</template>

<style scoped lang="scss">
@use '../../styles/app-button';

.app-button--windows {
  --button-accent: #35b6ff;
}

.app-button--android {
  --button-accent: #00f0a8;
}
</style>
