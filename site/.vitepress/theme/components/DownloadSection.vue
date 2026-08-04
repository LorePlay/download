<script setup>
import { computed, onMounted, ref } from 'vue';
import { withBase } from 'vitepress';
import AppButton from './ui/AppButton.vue';

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

function normalizeVersion(version) {
  if (typeof version !== 'string') {
    return '';
  }

  const trimmedVersion = version.trim();

  if (!trimmedVersion) {
    return '';
  }

  return `v${trimmedVersion.replace(/^v/i, '')}`;
}

function normalizeDownloadUrl(url) {
  if (typeof url !== 'string') {
    return '#';
  }

  const trimmedUrl = url.trim();

  return trimmedUrl || '#';
}

const manifestUrl = withBase('/channels/stable.json');
const brandMarkUrl = withBase('/assets/app-icon.png');
const manifest = ref(null);

async function loadManifest() {
  try {
    const response = await fetch(manifestUrl, { cache: 'no-store' });

    if (!response.ok) {
      return;
    }

    manifest.value = await response.json();
  } catch {
    manifest.value = null;
  }
}

const versionLabel = computed(() => {
  return (
    normalizeVersion(manifest.value?.appVersion) ||
    normalizeVersion(manifest.value?.releaseTag) ||
    'v0.0.0'
  );
});
const windowsUrl = computed(() => {
  return normalizeDownloadUrl(
    manifest.value?.desktop?.windowsUrl ??
      manifest.value?.desktop?.releasePageUrl,
  );
});
const androidUrl = computed(() => {
  return normalizeDownloadUrl(manifest.value?.android?.apkUrl);
});

onMounted(() => {
  void loadManifest();
});
</script>

<template>
  <header class="download-section" @click="stopBackgroundClick">
    <div
      class="download-section__glow download-section__glow--primary"
      aria-hidden="true"
    ></div>
    <div
      class="download-section__glow download-section__glow--secondary"
      aria-hidden="true"
    ></div>
    <div class="download-section__grid" aria-hidden="true"></div>
    <div class="download-section__content">
      <div class="download-section__brand">
        <img
          class="download-section__brand-mark"
          :src="brandMarkUrl"
          alt="Logo LorePlay"
          width="512"
          height="512"
          decoding="async"
        />
        <p class="download-section__brand-wordmark">LorePlay</p>
        <p class="download-section__tagline">
          Czat z wieloma postaciami. Model
          <span>Bielik</span> działa lokalnie, przez Ollamę.
        </p>
      </div>

      <div class="download-section__actions">
        <p class="download-section__prompt">
          <span>Pobierz</span>
          <span class="download-section__prompt-version">{{ versionLabel }}</span>
        </p>
        <AppButton
          :href="windowsUrl"
          aria-label="Pobierz na Windows"
          label="Windows"
          variant="windows"
        >
          <template #icon>
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M2 3.5 11 2v9H2v-7.5Zm10 7.5V1.86L22 0v11H12Zm-10 2H11v9l-9-1.5V13Zm10 0H22v11l-10-1.86V13Z"
              />
            </svg>
          </template>
        </AppButton>

        <AppButton
          :href="androidUrl"
          aria-label="Pobierz na Android"
          label="Android"
          variant="android"
        >
          <template #icon>
            <svg viewBox="0 0 576 512" aria-hidden="true">
              <path
                d="M420.5 253.9a24 24 0 1 1 0 48 24 24 0 1 1 0-48zm-265.1 0a24 24 0 1 1 0 48 24 24 0 1 1 0-48zm273.7-96.5l47.9-83c.8-1.1 1.3-2.4 1.5-3.8s.2-2.7-.1-4.1-.9-2.6-1.7-3.7-1.8-2-3-2.7-2.5-1.1-3.9-1.3-2.7 0-4 .4-2.5 1.1-3.6 1.9-1.9 2-2.5 3.2l-48.5 84.1c-38.8-17.4-80.8-26.4-123.3-26.4s-84.5 9-123.3 26.4L116.2 64.4c-.6-1.2-1.5-2.3-2.5-3.2s-2.3-1.5-3.6-1.9-2.7-.5-4-.4-2.7 .6-3.9 1.3-2.2 1.6-3 2.7-1.4 2.4-1.7 3.7-.3 2.7-.1 4.1 .8 2.6 1.5 3.8l47.9 83C64.5 202.2 8.2 285.5 0 384l576 0c-8.2-98.5-64.5-181.8-146.9-226.6z"
              />
            </svg>
          </template>
        </AppButton>
      </div>

      <p class="download-section__footer">Vxid Functixn</p>
    </div>
  </header>
</template>

<style scoped lang="scss">
.download-section {
  --hero-padding-inline: var(--space-l);
  --hero-padding-block: var(--space-xl);
  --hero-glow-size: clamp(20rem, 55vmax, 52rem);

  position: relative;
  display: flex;
  min-height: 100dvh;
  overflow: hidden;
  padding: var(--hero-padding-block) var(--hero-padding-inline);
  background:
    radial-gradient(circle at 50% -5%, rgba(0, 240, 168, 0.28), transparent 55%),
    radial-gradient(circle at 12% 88%, rgba(255, 61, 129, 0.18), transparent 45%),
    radial-gradient(circle at 88% 20%, rgba(53, 182, 255, 0.18), transparent 45%),
    linear-gradient(180deg, #0d1014 0%, #141a22 55%, #0d1014 100%);

  &__glow {
    position: absolute;
    width: var(--hero-glow-size);
    aspect-ratio: 1;
    border-radius: 50%;
    filter: blur(3rem);
    opacity: 0.5;
    pointer-events: none;

    &--primary {
      top: calc(var(--hero-glow-size) * -0.45);
      left: calc(var(--hero-glow-size) * -0.25);
      background: radial-gradient(
        circle,
        rgba(0, 240, 168, 0.5),
        transparent 65%
      );
    }

    &--secondary {
      right: calc(var(--hero-glow-size) * -0.4);
      bottom: calc(var(--hero-glow-size) * -0.35);
      background: radial-gradient(
        circle,
        rgba(53, 182, 255, 0.38),
        transparent 65%
      );
    }
  }

  &__grid {
    position: absolute;
    inset: 0;
    pointer-events: none;
    opacity: 0.35;
    background-image:
      linear-gradient(to right, rgba(255, 255, 255, 0.035) 1px, transparent 1px),
      linear-gradient(to bottom, rgba(255, 255, 255, 0.035) 1px, transparent 1px);
    background-size: 4rem 4rem;
    mask-image: radial-gradient(ellipse at 50% 35%, #000 15%, transparent 72%);
  }

  &__content {
    position: relative;
    z-index: 1;
    display: flex;
    flex: 1;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: var(--space-xl);
    width: min(100%, 48rem);
    margin: 0 auto;
    text-align: center;
  }

  &__brand {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--space-m);
  }

  &__brand-mark {
    display: block;
    width: clamp(7rem, 20vw, 12rem);
    height: auto;
    object-fit: contain;
    filter: drop-shadow(0 1rem 2.5rem rgba(0, 232, 168, 0.28));
  }

  &__brand-wordmark {
    margin: 0;
    font-size: clamp(2.5rem, 8vw, 4.5rem);
    font-weight: 600;
    line-height: 1;
    letter-spacing: -0.045em;
    color: var(--color-text);
  }

  &__tagline {
    max-width: 34rem;
    margin: 0;
    font-size: 1.05rem;
    line-height: 1.6;
    color: var(--color-text-muted);
    text-wrap: balance;

    span {
      color: var(--color-accent);
      font-weight: 600;
    }
  }

  &__actions {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: var(--space-s);
  }

  &__prompt,
  &__footer {
    margin: 0;
    font-family: var(--font-mono);
    letter-spacing: 0.24em;
    text-transform: uppercase;
    color: var(--color-text-muted);
  }

  &__prompt {
    display: inline-flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    flex-basis: 100%;
    gap: var(--space-xs);
    font-size: 0.75rem;
  }

  &__prompt-version {
    padding: var(--space-2xs) var(--space-xs);
    border: 1px solid var(--color-border-strong);
    border-radius: var(--radius-xs);
    letter-spacing: 0.05em;
    text-transform: none;
    color: var(--color-accent);
    background: var(--color-surface-strong);
  }

  &__footer {
    margin-top: auto;
    font-size: 0.7rem;
    opacity: 0.7;
  }
}

@media (max-width: 40rem) {
  .download-section {
    --hero-padding-inline: var(--space-m);
    --hero-padding-block: var(--space-l);
  }
}
</style>
