<script setup lang="ts">
import { RouterView, RouterLink, useRoute } from "vue-router";
import InstallPrompt from "./components/InstallPrompt.vue";

const route = useRoute();

function isActive(path: string) {
  return route.path === path;
}
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-white">
    <div class="mx-auto flex min-h-screen max-w-md flex-col bg-slate-950">
      <!-- Header móvil -->
      <header
        class="sticky top-0 z-10 border-b border-slate-800 bg-slate-950/90 px-5 py-4 backdrop-blur"
      >
        <p class="text-sm text-slate-400">Mi PWA</p>

        <h1 class="text-xl font-bold">App móvil Vue</h1>
      </header>

      <!-- Contenido -->
      <main class="flex-1 px-5 py-6 pb-24">
        <RouterView />
      </main>

      <!-- Prompt de instalación PWA -->
      <InstallPrompt />

      <!-- Bottom Navigation -->
      <nav
        class="fixed bottom-0 left-1/2 z-20 w-full max-w-md -translate-x-1/2 border-t border-slate-800 bg-slate-950/95 px-4 py-3 backdrop-blur"
      >
        <div class="grid grid-cols-3 gap-2">
          <RouterLink
            to="/"
            class="flex flex-col items-center rounded-2xl px-3 py-2 text-xs transition"
            :class="
              isActive('/')
                ? 'bg-blue-500 text-white'
                : 'text-slate-400 hover:bg-slate-800'
            "
          >
            <span class="text-xl">🏠</span>
            <span>Inicio</span>
          </RouterLink>

          <RouterLink
            to="/dashboard"
            class="flex flex-col items-center rounded-2xl px-3 py-2 text-xs transition"
            :class="
              isActive('/dashboard')
                ? 'bg-blue-500 text-white'
                : 'text-slate-400 hover:bg-slate-800'
            "
          >
            <span class="text-xl">📊</span>
            <span>Panel</span>
          </RouterLink>

          <RouterLink
            to="/about"
            class="flex flex-col items-center rounded-2xl px-3 py-2 text-xs transition"
            :class="
              isActive('/about')
                ? 'bg-blue-500 text-white'
                : 'text-slate-400 hover:bg-slate-800'
            "
          >
            <span class="text-xl">👤</span>
            <span>Perfil</span>
          </RouterLink>
        </div>
      </nav>
    </div>
  </div>
</template>
