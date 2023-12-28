<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import type { Artist } from '../types/Artist.ts'
import ArtistCard from '../components/ArtistCard.vue'
import { getAccessToken } from '../utils/getAccessToken.ts'

const search = ref('')
const artists: Ref<
  Array<Artist>
> = ref([])
const isLoading = ref(false)

const getArtists = async () => {
  const response = await fetch(
    `https://api.spotify.com/v1/search?q=${search.value}&type=artist&limit=20`,
    {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('accessToken')}`
      }
    }
  )
  const json = await response.json()
  return json
}

const onSearch = async () => {
  isLoading.value = true
  let data = await getArtists()
  if (data.error?.status === 401) {
    await getAccessToken()
    data = await getArtists()
  }
  artists.value = data.artists.items
  isLoading.value = false
}
</script>

<template>
  <div class="App">
    <input
      class="Input"
      placeholder="Ex: Amy Winehouse"
      v-model="search"
      @input="
        (event) => {
          search = (event.target as HTMLInputElement)?.value
        }
      "
      @keydown="
        (event) => {
          if (event.key === 'Enter' && search) {
            onSearch()
          }
        }
      "
    />
    <button class="Button" @click="onSearch" :disabled="!search || isLoading">
      {{ isLoading ? 'Loading...' : 'Search' }}
    </button>
    <h1 class="Artists-title">
      {{ artists.length ? 'Artists' : 'Find your favourite artists' }}
    </h1>
    <div class="Grid">
      <div v-for="artist in artists" :key="artist.id">
        <ArtistCard :artist="artist" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.Input {
  height: 30px;
  padding: 12px;
  border-radius: 20px;
  border: none;
  margin-right: 12px;
  width: 300px;
}

.Button {
  background-color: black;
  border: 1px solid white;
  border-radius: 22px;
  color: white;
  cursor: pointer;
  height: 30px;
  padding: 6px 8px;
  transition:
    background-color 0.3s,
    border 0.3s,
    color 0.3s;
}

a {
  text-decoration: none;
}

.Button:hover {
  background-color: white;
  color: black;
  border: 1px solid black;
}

.App {
  text-align: center;
}

.Artists-title {
  color: white;
}
</style>
