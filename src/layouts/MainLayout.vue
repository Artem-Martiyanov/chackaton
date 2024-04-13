<script setup lang="ts">
defineOptions({
  name: 'MainLayout'
});
</script>

<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          Исследование и прогнозирование динамики лояльности участников НПО
        </q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>


    <q-footer>
      <q-toolbar>
        <q-toolbar-title class="text-right">
          "Братья навсегда", 2024
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>

  </q-layout>
</template>
