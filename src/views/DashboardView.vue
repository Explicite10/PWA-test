<script setup lang="ts">
import { useCounterStore } from "../stores/counterStore";

const counterStore = useCounterStore();
</script>

<template>
  <section class="space-y-6">
    <div>
      <h2 class="text-3xl font-bold">Dashboard</h2>

      <p class="mt-2 text-slate-400">Estado global manejado con Pinia.</p>
    </div>

    <div class="rounded-4xl border border-slate-800 bg-slate-900 p-6">
      <p class="text-sm uppercase tracking-wide text-slate-400">Contador</p>

      <div class="mt-6 flex items-end justify-between">
        <h3 class="text-7xl font-bold">
          {{ counterStore.count }}
        </h3>

        <div class="rounded-2xl bg-blue-500/10 px-4 py-2 text-blue-400">
          x2: {{ counterStore.doubleCount }}
        </div>
      </div>

      <div class="mt-8 grid grid-cols-3 gap-3">
        <button
          class="rounded-2xl bg-slate-800 py-4 font-bold active:scale-95"
          @click="counterStore.decrement"
        >
          -
        </button>

        <button
          class="rounded-2xl bg-blue-500 py-4 font-bold active:scale-95"
          @click="counterStore.increment"
        >
          +
        </button>

        <button
          class="rounded-2xl bg-slate-800 py-4 font-bold active:scale-95"
          @click="counterStore.reset"
        >
          Reset
        </button>
      </div>
    </div>

    <div class="grid grid-cols-2 gap-4">
      <div class="rounded-3xl bg-slate-900 p-5">
        <p class="text-sm text-slate-400">Estado</p>
        <p class="mt-2 text-2xl font-bold">Activo</p>
      </div>

      <div class="rounded-3xl bg-slate-900 p-5">
        <p class="text-sm text-slate-400">Modo</p>
        <p class="mt-2 text-2xl font-bold">PWA</p>
      </div>
    </div>
  </section>
</template>
