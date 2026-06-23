<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";

let deferredPrompt: any = null;

const canInstall = ref(false);   // Android/Chrome: prompt nativo disponible
const isIos = ref(false);        // iOS Safari: mostrar instrucciones manuales
const iosDismissed = ref(false); // El usuario cerró el banner de iOS
const installed = ref(false);

function detectIos() {
  const ua = navigator.userAgent;
  // iPad en iOS 13+ se reporta como Macintosh, se distingue por touchPoints
  return /iphone|ipad|ipod/i.test(ua) || (navigator.maxTouchPoints > 1 && /Macintosh/i.test(ua));
}

function onBeforeInstallPrompt(e: Event) {
  e.preventDefault();
  deferredPrompt = e;
  canInstall.value = true;
}

function onAppInstalled() {
  canInstall.value = false;
  installed.value = true;
  deferredPrompt = null;
}

async function install() {
  if (!deferredPrompt) return;
  deferredPrompt.prompt();
  const { outcome } = await deferredPrompt.userChoice;
  if (outcome === "accepted") {
    canInstall.value = false;
  }
  deferredPrompt = null;
}

onMounted(() => {
  const isStandalone =
    window.matchMedia("(display-mode: standalone)").matches ||
    (navigator as any).standalone === true; // propiedad propia de Safari/iOS

  if (isStandalone) {
    installed.value = true;
    return;
  }

  if (detectIos()) {
    isIos.value = true;
  } else {
    window.addEventListener("beforeinstallprompt", onBeforeInstallPrompt);
    window.addEventListener("appinstalled", onAppInstalled);
  }
});

onBeforeUnmount(() => {
  window.removeEventListener("beforeinstallprompt", onBeforeInstallPrompt);
  window.removeEventListener("appinstalled", onAppInstalled);
});
</script>

<template>
  <!-- Android / Chrome: prompt nativo -->
  <Transition name="slide-up">
    <div
      v-if="canInstall && !installed"
      class="fixed bottom-20 left-1/2 z-30 w-full max-w-md -translate-x-1/2 px-4"
    >
      <div
        class="flex items-center justify-between gap-4 rounded-3xl border border-slate-700 bg-slate-900 p-4 shadow-2xl"
      >
        <div class="flex items-center gap-3">
          <span class="text-2xl">📲</span>
          <div>
            <p class="text-sm font-semibold">Instalar app</p>
            <p class="text-xs text-slate-400">Agregar a la pantalla de inicio</p>
          </div>
        </div>
        <button
          @click="install"
          class="rounded-2xl bg-blue-500 px-4 py-2 text-sm font-semibold text-white transition hover:bg-blue-400 active:scale-95"
        >
          Instalar
        </button>
      </div>
    </div>
  </Transition>

  <!-- iOS / Safari: instrucciones manuales -->
  <Transition name="slide-up">
    <div
      v-if="isIos && !installed && !iosDismissed"
      class="fixed bottom-20 left-1/2 z-30 w-full max-w-md -translate-x-1/2 px-4"
    >
      <div
        class="relative rounded-3xl border border-slate-700 bg-slate-900 p-5 shadow-2xl"
      >
        <!-- Botón cerrar -->
        <button
          @click="iosDismissed = true"
          class="absolute right-4 top-4 text-slate-500 hover:text-slate-300"
          aria-label="Cerrar"
        >
          ✕
        </button>

        <div class="flex items-center gap-3">
          <span class="text-2xl">📲</span>
          <p class="text-sm font-semibold">Instalar en iPhone / iPad</p>
        </div>

        <ol class="mt-4 space-y-3 text-sm text-slate-300">
          <li class="flex items-start gap-3">
            <span class="mt-0.5 flex h-6 w-6 shrink-0 items-center justify-center rounded-full bg-blue-500 text-xs font-bold text-white">1</span>
            <span>Toca el botón <strong class="text-white">Compartir</strong> <span class="inline-block rounded bg-slate-700 px-1.5 py-0.5 font-mono text-xs">⬆</span> en la barra de Safari</span>
          </li>
          <li class="flex items-start gap-3">
            <span class="mt-0.5 flex h-6 w-6 shrink-0 items-center justify-center rounded-full bg-blue-500 text-xs font-bold text-white">2</span>
            <span>Desplázate y selecciona <strong class="text-white">"Agregar a pantalla de inicio"</strong></span>
          </li>
          <li class="flex items-start gap-3">
            <span class="mt-0.5 flex h-6 w-6 shrink-0 items-center justify-center rounded-full bg-blue-500 text-xs font-bold text-white">3</span>
            <span>Toca <strong class="text-white">"Agregar"</strong> en la esquina superior derecha</span>
          </li>
        </ol>

        <!-- Flecha apuntando hacia abajo (indica la barra de Safari) -->
        <div class="mt-4 flex justify-center">
          <span class="animate-bounce text-blue-400 text-xl">↓</span>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.3s ease;
}
.slide-up-enter-from,
.slide-up-leave-to {
  opacity: 0;
  transform: translate(-50%, 1rem);
}
</style>
