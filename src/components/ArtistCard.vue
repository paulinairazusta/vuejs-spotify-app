<script setup lang="ts">
import type { Artist } from '../types/Artist.ts'
import avatar from '../assets/avatar.jpg'

const props = defineProps<{
  artist: Artist
}>()
</script>

<template>
  <router-link :to="{ name: 'Artist', params: { id: artist?.id } }">
    <div class="Card">
      <img :src="props.artist?.images[2]?.url ?? avatar" alt="Artist avatar" class="Card-image" />
      <p class="Card-name">{{ props.artist?.name }}</p>
      <span v-if="props.artist?.genres[0]" class="Card-tag">{{ props.artist.genres[0] }}</span>
    </div>
  </router-link>
</template>

<style scoped>
.Card {
  background-color: rgb(24, 24, 24);
  border-radius: 12px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  max-height: 209px;
  max-width: 144px;
  padding: 12px;
  position: relative;
  text-align: center;
  transition: background-color 0.3s;
}

.Card:hover {
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

.Card-tag {
  background-color: rgb(18, 18, 18);
  border: 1px solid white;
  border-radius: 12px;
  color: white;
  font-size: 10px;
  line-height: 10px;
  padding: 2px 8px;
  position: absolute;
  right: 14px;
  top: 115px;
}
</style>
