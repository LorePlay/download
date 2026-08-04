<script setup>
import AppHeading from './ui/AppHeading.vue';
import SectionCard from './ui/SectionCard.vue';

function stopBackgroundClick(event) {
  const targetElement = event.target;

  if (!(targetElement instanceof Element)) {
    return;
  }

  if (
    targetElement.closest(
      'a, button, input, select, textarea, summary, label, [role="button"], [role="link"]',
    )
  ) {
    return;
  }

  event.stopPropagation();
}

const sectionParagraphs = [
  'LorePlay to klient czatu z wieloma postaciami, w którym każda z nich ma własny opis, styl wypowiedzi i pamięć rozmowy — a użytkownik prowadzi je równolegle w osobnych sesjach.',
  'Model językowy Bielik działa lokalnie przez Ollamę, więc rozmowy nie opuszczają komputera. Nie ma kont, kluczy API ani limitów — wystarczy uruchomiona Ollama i pobrany model.',
  'Landing pełni rolę prostego punktu wejścia do aplikacji: pokazuje, czym jest projekt, i pozwala szybko pobrać wersję dla swojego systemu.',
];
</script>

<template>
  <main class="main-content" @click="stopBackgroundClick">
    <SectionCard>
      <AppHeading eyebrow="O projekcie" tag="h2" variant="section">
        Twoje postacie, <span>twój sprzęt</span>
      </AppHeading>

      <p
        v-for="paragraph in sectionParagraphs"
        :key="paragraph"
        class="main-content__text"
      >
        {{ paragraph }}
      </p>
    </SectionCard>
  </main>
</template>

<style scoped lang="scss">
:global(*),
:global(*::before),
:global(*::after) {
  box-sizing: border-box;
}

:global(html) {
  color-scheme: dark;
}

:global(body) {
  --color-background: #0d1014;
  --color-surface: rgba(24, 30, 39, 0.72);
  --color-surface-strong: rgba(24, 30, 39, 0.95);
  --color-border: rgba(255, 255, 255, 0.09);
  --color-border-strong: rgba(255, 255, 255, 0.16);
  --color-text: #f2f6f8;
  --color-text-muted: #a6b2bd;
  --color-accent: #00f0a8;
  --color-accent-alt: #35b6ff;
  --color-accent-hot: #ff3d81;
  --page-max-width: 64rem;
  --font-mono: 'JetBrains Mono', 'Cascadia Mono', 'Consolas', monospace;
  --radius-xs: 0.25rem;
  --radius-s: 0.5rem;
  --radius-m: 0.875rem;
  --radius-pill: 999px;
  --space-2xs: 0.25rem;
  --space-xs: 0.5rem;
  --space-s: 0.75rem;
  --space-m: 1.25rem;
  --space-l: 2rem;
  --space-xl: 3rem;
  --space-2xl: 5rem;
  --transition-base: 160ms ease;

  margin: 0;
  min-width: 20rem;
  font-family: 'Roboto', 'Segoe UI', system-ui, sans-serif;
  color: #f2f6f8;
  background: #0d1014;
}

.main-content {
  padding: var(--space-2xl) var(--space-l);
  border-top: 1px solid var(--color-border);
}

.main-content__text {
  margin: 0;
  max-width: 46rem;
  font-size: 1rem;
  line-height: 1.75;
  color: var(--color-text-muted);
}

@media (max-width: 40rem) {
  .main-content {
    padding: var(--space-xl) var(--space-m);
  }
}
</style>
