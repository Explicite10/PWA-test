<script setup lang="ts">
import { RouterLink } from "vue-router";
</script>

<template>
  <header class="border-b border-slate-800 bg-slate-950 text-white">
    <nav class="mx-auto flex max-w-6xl items-center justify-between px-6 py-4">
      <RouterLink to="/" class="text-xl font-bold"> Vue PWA </RouterLink>

      <div class="flex gap-4 text-sm">
        <RouterLink to="/" class="rounded-lg px-3 py-2 hover:bg-slate-800">
          Inicio
        </RouterLink>

        <RouterLink
          to="/dashboard"
          class="rounded-lg px-3 py-2 hover:bg-slate-800"
        >
          Dashboard
        </RouterLink>

        <RouterLink to="/about" class="rounded-lg px-3 py-2 hover:bg-slate-800">
          Acerca
        </RouterLink>
      </div>
    </nav>
  </header>
</template>
