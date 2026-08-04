<template>
  <section class="section-card">
    <div class="section-card__inner">
      <slot />
    </div>
  </section>
</template>

<style scoped lang="scss">
.section-card {
  width: min(100%, var(--page-max-width));
  margin: 0 auto;

  &__inner {
    position: relative;
    display: flex;
    flex-direction: column;
    gap: var(--space-m);
    padding: var(--space-xl);
    border: 1px solid var(--color-border);
    border-radius: var(--radius-m);
    background: var(--color-surface);

    &::before {
      content: '';
      position: absolute;
      inset-block: var(--space-xl);
      inset-inline-start: -1px;
      width: 2px;
      border-radius: var(--radius-pill);
      background: linear-gradient(
        180deg,
        var(--color-accent),
        transparent 85%
      );
    }
  }
}

@media (max-width: 40rem) {
  .section-card__inner {
    padding: var(--space-l);

    &::before {
      inset-block: var(--space-l);
    }
  }
}
</style>
