<script setup lang="ts">
import { ref, onBeforeMount } from 'vue'
import type { Ref } from 'vue'
import { useRoute } from 'vue-router'
import type { Track } from '../types/Track.ts'
import { getAccessToken } from '../utils/getAccessToken.ts'

const tracks: Ref<Array<Track>> = ref([])
const route = useRoute()

onBeforeMount(() => {
  async function getAuthorizedTracks() {
    async function getTracks() {
      const response = await fetch(`https://api.spotify.com/v1/albums/${route.query.id}/tracks`, {
        headers: {
          Authorization: `Bearer ${localStorage.getItem('accessToken')}`
        }
      })
      const json = await response.json()
      return json
    }
    let data = await getTracks()
    if (data.error?.status === 401) {
      await getAccessToken()
      data = await getTracks()
    }
    tracks.value = data.items
  }
  getAuthorizedTracks()
})
</script>

<template>
  <main class="Page-container">
    <div class="Album-container">
      <div>
        <img :src="route.query.image || avatar" alt="Album cover" class="Album-cover" />
        <div class="Album-info">
          <p>{{ route.query.name }}</p>
          <p class="Album-date">Release date: {{ route.query.release_date }}</p>
        </div>
      </div>
      <div>
        <ol>
          <li v-for="track in tracks" :key="track.id" class="Track">
            <p>{{ track.name }}</p>
          </li>
        </ol>
      </div>
    </div>
  </main>
</template>

<style scoped>
.Page-container {
  align-items: center;
  justify-content: center;
  display: flex;
  min-height: 100vh;
}

.Album-container {
  display: flex;
  gap: 40px;
  margin: 80px;
}

.Album-info {
  color: white;
  font-size: 32px;
  font-weight: 600;
  text-align: center;
}

.Album-date {
  font-size: 20px;
  font-weight: 400;
}

.Album-cover {
  height: 540px;
  width: 540px;
}

.Track {
  color: white;
  font-size: 18px;
}
</style>
