<script setup lang="ts">
import type { Album } from '../types/Album.ts'
import avatar from '../assets/avatar.jpg'

const props = defineProps<{
  album: Album
}>()
</script>

<template>
  <router-link :to="{ name: 'Album', query: { id: album.id, image: album.images[0]?.url || '', name: album.name, release_date: album.release_date } }">
    <div class="Album-card">
      <img :src="props.album?.images[1]?.url ?? avatar" alt="Album avatar" class="Card-image" />
      <p class="Card-name">{{ props.album?.name }}</p>
    </div>
  </router-link>
</template>

<style scoped>
.Album-card {
  background-color: rgb(24, 24, 24);
  border-radius: 12px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  max-height: 190px;
  max-width: 144px;
  padding: 12px;
  position: relative;
  text-align: center;
  transition: background-color 0.3s;
}

.Album-card:hover {
  background-color: rgb(40, 40, 40);
}

.Card-image {
  border-radius: 8px;
  height: 120px;
  width: 120px;
}

.Card-name {
  color: white;
  flex: 1;
  font-size: 14px;
  text-overflow: ellipsis;
}
</style>
