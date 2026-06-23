<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";

// El evento nativo del navegador que dispara el prompt de instalación
let deferredPrompt: any = null;

const canInstall = ref(false);
const installed = ref(false);

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
  // Si ya está corriendo como PWA instalada, no mostrar nada
  if (window.matchMedia("(display-mode: standalone)").matches) {
    installed.value = true;
    return;
  }
  window.addEventListener("beforeinstallprompt", onBeforeInstallPrompt);
  window.addEventListener("appinstalled", onAppInstalled);
});

onBeforeUnmount(() => {
  window.removeEventListener("beforeinstallprompt", onBeforeInstallPrompt);
  window.removeEventListener("appinstalled", onAppInstalled);
});
</script>

<template>
  <!-- Baner de instalación — solo visible si el navegador lo permite y aún no está instalada -->
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
