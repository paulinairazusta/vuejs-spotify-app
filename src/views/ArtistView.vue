<script setup lang="ts">
import { ref, onBeforeMount } from 'vue'
import type { Ref } from 'vue'
import type { Album } from '../types/Album.ts'
import { useRoute } from 'vue-router'
import AlbumCard from '../components/AlbumCard.vue'
import { getAccessToken } from '../utils/getAccessToken.ts'

const albums: Ref<Array<Album>> = ref([])
const route = useRoute()

onBeforeMount(() => {
  async function getAuthorizedAlbums() {
    async function getAlbums() {
      const response = await fetch(`https://api.spotify.com/v1/artists/${route.params.id}/albums`, {
        headers: {
          Authorization: `Bearer ${localStorage.getItem('accessToken')}`
        }
      })
      const json = await response.json()
      return json
    }
    let data = await getAlbums()
    if (data.error?.status === 401) {
      await getAccessToken()
      data = await getAlbums()
    }
    albums.value = data.items
  }
  getAuthorizedAlbums()
})
</script>

<template>
  <div class="Albums-container">
    <h1 class="Albums-title">Albums</h1>
    <div class="Grid">
      <div v-for="album in albums" :key="album.id">
        <AlbumCard :album="album" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.Albums-title {
  color: white;
  margin: 30px;
}

.Albums-container {
  text-align: center;
}
</style>
