<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header class="App-header">
      <nav>
        <RouterLink to="/">
          <h2 class="App-header-title">My Spotify App</h2>
        </RouterLink>
      </nav>
  </header>

  <RouterView />
</template>

<style scoped>
.App-header {
  background-color: white;
  margin-bottom: 20px;
  padding: 20px;
  text-align: left;
}

.App-header-title {
  margin: 0;
}

a {
  color: rgb(18, 18, 18);
}
</style>
