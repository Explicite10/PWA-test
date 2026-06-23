<template>
  <section class="space-y-6">
    <!-- Card principal -->
    <div
      class="rounded-4xl bg-linear-to-br from-blue-500 to-indigo-600 p-6 shadow-xl"
    >
      <p class="text-sm font-medium text-blue-100">Bienvenido</p>

      <h2 class="mt-3 text-3xl font-bold">Tu app PWA está lista</h2>

      <p class="mt-3 text-blue-100">
        Instálala en tu celular y úsala como una aplicación móvil.
      </p>

      <button
        class="mt-6 rounded-2xl bg-white px-5 py-3 font-semibold text-blue-600"
      >
        Comenzar
      </button>
    </div>

    <!-- Acciones rápidas -->
    <div>
      <h3 class="text-lg font-bold">Acciones rápidas</h3>

      <div class="mt-4 grid grid-cols-2 gap-4">
        <RouterLink
          to="/dashboard"
          class="rounded-3xl border border-slate-800 bg-slate-900 p-5"
        >
          <span class="text-3xl">📊</span>
          <h4 class="mt-4 font-bold">Dashboard</h4>
          <p class="mt-1 text-sm text-slate-400">Ver estado global</p>
        </RouterLink>

        <RouterLink
          to="/about"
          class="rounded-3xl border border-slate-800 bg-slate-900 p-5"
        >
          <span class="text-3xl">⚙️</span>
          <h4 class="mt-4 font-bold">Ajustes</h4>
          <p class="mt-1 text-sm text-slate-400">Configuración app</p>
        </RouterLink>
      </div>
    </div>

    <!-- Lista tipo app -->
    <div class="rounded-3xl border border-slate-800 bg-slate-900 p-5">
      <h3 class="font-bold">Características</h3>

      <div class="mt-4 space-y-4">
        <div class="flex items-center gap-4">
          <div
            class="flex h-12 w-12 items-center justify-center rounded-2xl bg-blue-500/20"
          >
            ⚡
          </div>

          <div>
            <p class="font-semibold">Rápida</p>
            <p class="text-sm text-slate-400">Carga optimizada con Vite</p>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <div
            class="flex h-12 w-12 items-center justify-center rounded-2xl bg-green-500/20"
          >
            📲
          </div>

          <div>
            <p class="font-semibold">Instalable</p>
            <p class="text-sm text-slate-400">Funciona como app móvil</p>
          </div>
        </div>

        <div class="flex items-center gap-4">
          <div
            class="flex h-12 w-12 items-center justify-center rounded-2xl bg-purple-500/20"
          >
            🌐
          </div>

          <div>
            <p class="font-semibold">PWA</p>
            <p class="text-sm text-slate-400">Service Worker y manifest</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
