<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <Suspense>
    <template #default>
      <main>
        <RouterView />
      </main>
    </template>
  </Suspense>
</template>

<style scoped>
main {
  display: grid;
  place-items: center;
  border-radius: 24px;
  width: 95vw;
  max-width: 464px;
}
</style>
