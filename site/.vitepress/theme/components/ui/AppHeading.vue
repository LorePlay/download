<script setup>
defineProps({
  eyebrow: {
    type: String,
    default: '',
  },
  badge: {
    type: String,
    default: '',
  },
  tag: {
    type: String,
    default: 'h2',
  },
  variant: {
    type: String,
    default: 'section',
  },
});
</script>

<template>
  <div class="app-heading" :class="`app-heading--${variant}`">
    <p v-if="eyebrow" class="app-heading__eyebrow">{{ eyebrow }}</p>
    <div class="app-heading__title-row">
      <component :is="tag" class="app-heading__title">
        <slot />
      </component>
      <span v-if="badge" class="app-heading__badge">{{ badge }}</span>
    </div>
  </div>
</template>

<style scoped lang="scss">
.app-heading {
  display: grid;
  gap: var(--space-xs);

  &__eyebrow,
  &__title {
    margin: 0;
  }

  &__eyebrow {
    display: inline-flex;
    align-items: center;
    gap: var(--space-xs);
    font-family: var(--font-mono);
    font-size: 0.75rem;
    font-weight: 600;
    letter-spacing: 0.24em;
    text-transform: uppercase;
    color: var(--color-accent);

    &::before {
      content: '';
      width: 1.5rem;
      height: 1px;
      background: var(--color-accent);
    }
  }

  &__title-row {
    display: flex;
    flex-wrap: wrap;
    align-items: baseline;
    gap: var(--space-s);
  }

  &__title {
    font-weight: 600;
    letter-spacing: -0.02em;
    color: var(--color-text);

    :deep(span) {
      color: var(--color-accent);
    }
  }

  &__badge {
    display: inline-flex;
    align-items: center;
    padding: var(--space-2xs) var(--space-xs);
    border: 1px solid var(--color-border-strong);
    border-radius: var(--radius-xs);
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--color-text-muted);
    background: var(--color-surface-strong);
  }

  &--hero {
    justify-items: center;
    text-align: center;

    .app-heading__title-row {
      justify-content: center;
    }

    .app-heading__title {
      font-size: clamp(2.5rem, 7vw, 4.5rem);
      line-height: 1;
      letter-spacing: -0.04em;
    }
  }

  &--section {
    .app-heading__title {
      font-size: clamp(1.6rem, 3.4vw, 2.4rem);
      line-height: 1.15;
    }
  }
}
</style>
