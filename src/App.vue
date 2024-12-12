<script setup lang='ts'>
import { ref, computed } from 'vue';
import artists from './assets/artists.json';
import songs from './assets/songs.json';

const genres = songs
    .filter(
        (value, index, array) =>
            array.findIndex((s) => s.GENRE == value.GENRE) === index
    )
    .map((s) => s.GENRE)
    .sort();

const artistInput = ref('');
const songInput = ref("");
const selectedGenre = ref("");

const filteredSongs = computed(() => {
    return songs.filter((s) => {
        const artistMatches = artistInput.value
            ? artists.some(
                  (a) =>
                      a._id === s.ARTIST_ID &&
                      a.artist
                          .toLowerCase()
                          .includes(artistInput.value.toLowerCase())
              )
            : true;
        const songMatches = songInput.value
            ? s.TITLE.toLowerCase().includes(songInput.value.toLowerCase())
            : true;
        const genreMatches = selectedGenre.value
            ? s.GENRE === selectedGenre.value
            : true;
        return artistMatches && songMatches && genreMatches;
    });
});

</script>

<template>

  <div class="filters">
      <input
          type="text"
          placeholder="Search for artist"
          v-model="artistInput" />
      <input
          type="text"
          placeholder="Search for song"
          v-model="songInput"/>
      <select v-model="selectedGenre">
          <option value="">All Genres</option>
          <option v-for="genre in genres" :key="genre" :value="genre">
              {{ genre }}
          </option>
      </select>
  </div>
  <table>
      <thead>
          <tr>
              <th>Title</th>
              <th>Artist</th>
              <th>Genre</th>
              <th>Year of release</th>
              <th>Duration</th>
          </tr>
      </thead>
      <tbody>
          <tr v-for="song in filteredSongs" :key="song._id">
              <td>{{ song.TITLE }}</td>
              <td>
                  {{
                      artists.find((artist) => artist._id === song.ARTIST_ID)
                          ?.artist
                  }}
              </td>
              <td>{{ song.GENRE }}</td>
              <td>{{ song.YEAR }}</td>
              <td>{{ song.TIME }}</td>
          </tr>
      </tbody>
  </table>
</template>

<style scoped>

div.filters {
  margin-top: 20px;
}

input[type="text"] {
  padding: 8px;
  border: 1px solid var(--eggplant);
  border-radius: 8px;
  background-color: var(--misty-rose);
  font-size: 14px;
  color: var(--eggplant);
}

select {
    padding: 8px;
    border: 1px solid var(--eggplant);
    border-radius: 8px;
    background-color: var(--eggplant);
    font-size: 14px;
    color: var(--misty-rose);
}

select {
    background-color: var(--eggplant);
    color: var(--misty-rose);    
}

input {
  margin-right: 5px;
  background-color: var(--misty-rose);
  color: var(--old-rose);
}

table {
    width: 80%;
    border-collapse: collapse;
    background-color: var(--bg-rosy-brown);
    border: 2px solid var(--eggplant);
    display: block;
    border-radius: 10px;
    color: var(--misty-rose);
}

th,
td {
    padding: 10px;
    text-align: left;
    width: 50%;
}
th {
    background-color: var(--eggplant);
    color: var(--old-rose);
}

tbody tr:nth-child(odd) {
    background-color: var(--bg-rosy-brown);
}
tbody tr:nth-child(even) {
    background-color: var(--eggplant);
}

th:first-of-type {
    border-top-left-radius: 5px;
}
th:last-of-type {
    border-top-right-radius: 5px;
}
</style>
