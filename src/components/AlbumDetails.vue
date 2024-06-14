<template>
    <div class="AlbumDetails-container">
      <h2 class="album-heading">Choose Album:</h2>
      <select v-model="selectedAlbum" @change="fetchPhotos" class="album-select">
        <option v-for="album in albums" :key="album.id" :value="album.id">{{ album.title }}</option>
      </select>
      <h2 class="album-heading">Photos in Album {{ selectedAlbum }}</h2>
      <div v-if="photos.length" class="photo-table">
        <table>
          <thead>
            <tr>
              <th>Thumbnail</th>
              <th>Title</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="photo in photos" :key="photo.id" class="photo-row" @click="showPhoto(photo.url)">
              <td><img :src="photo.thumbnailUrl" class="photo-thumbnail"></td>
              <td>{{ photo.title }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div v-else class="no-photos">
        <p>No photos available.</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useRoute } from 'vue-router'
  
  const route = useRoute()
  const selectedAlbum = ref(route.params.id)
  const albums = ref([])
  const photos = ref([])
  
  const fetchAlbums = async () => {
    try {
      const response = await fetch(`https://jsonplaceholder.typicode.com/albums`)
      albums.value = await response.json()
    } catch (error) {
      console.error('Error fetching albums:', error)
    }
  }
  
  const fetchPhotos = async () => {
    try {
      const response = await fetch(`https://jsonplaceholder.typicode.com/albums/${selectedAlbum.value}/photos`)
      photos.value = await response.json()
    } catch (error) {
      console.error('Error fetching photos:', error)
    }
  }
  
  const showPhoto = (url) => {
    window.open(url, '_blank')
  }
  
  onMounted(() => {
    fetchAlbums()
    fetchPhotos()
  })
  </script>
  
  <style scoped>
  .AlbumDetails-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-image: url('https://th.bing.com/th/id/OIP.s3fJbDvDCIxPaR_Cq7K8VwHaHa?w=1024&h=1024&rs=1&pid=ImgDetMain');
    background-size: cover;
    background-position: center;
    color: #fff;
  }
  
  .album-heading {
    font-size: 24px;
    text-align: center;
    margin-bottom: 10px;
  }
  
  .album-select {
    margin-bottom: 20px;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #007bff;
    border-radius: 5px;
    background-color: #fff;
    color: #007bff;
  }
  
  .photo-table {
    margin-top: 20px;
    width: 80%;
    max-width: 800px;
    overflow-x: auto;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
    background-color: #f5ca81;
    border-radius: 5px;
  }
  
  th, td {
    padding: 10px;
    text-align: center;
  }
  
  th {
    background-color: #007bff;
    color: #fff;
  }
  
  .photo-thumbnail {
    width: 100px;
    height: 100px;
    object-fit: cover;
    border: 1px solid #007bff;
    border-radius: 5px;
    cursor: pointer;
    transition: border-color 0.3s;
  }
  
  .photo-row:hover {
    background-color: rgba(0, 123, 255, 0.1);
  }
  
  .no-photos {
    text-align: center;
    margin-top: 20px;
  }
  </style>
  