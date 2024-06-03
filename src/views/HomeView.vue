<script>
import axios from 'axios';
import CharacterCard from '../components/CharacterCard.vue';
import PaginationComponent from '../components/PaginationComponent.vue';

export default {
  components: {
    CharacterCard,
    PaginationComponent
  },
  data() {
    return {
      characters: [],
      currentPage: 1,
      hasNextPage: true,
      filters: {
        name: '',
        status: ''
      }
    };
  },
  methods: {
    async fetchCharacters(page = 1) {
      this.currentPage = page;
      try {
        const response = await axios.get('https://rickandmortyapi.com/api/character', {
          params: {
            page,
            name: this.filters.name,
            status: this.filters.status
          }
        });
        this.characters = response.data.results;
        this.hasNextPage = response.data.info.next !== null;
      } catch (error) {
        if (error.response && error.response.status === 404) {
          this.hasNextPage = false;
        } else {
          console.error(error);
        }
      }
    },
    applyFilters() {
      this.fetchCharacters(1); 
    }
  },
  mounted() {
    this.fetchCharacters();
  }
}
</script>
<template>
  <div>
    <h1>Rick and Morty Characters</h1>
    <div>
      <input v-model="filters.name" placeholder="Name">
      <select v-model="filters.status">
        <option value="">Status</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters">Apply</button>
    </div>
    <div class="characters">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
    <PaginationComponent :currentPage="currentPage" @page-change="fetchCharacters" :hasNextPage="hasNextPage" />
  </div>
</template>



<style>
.characters {
  display: flex;
  flex-wrap: wrap;
  justify-content: center


}
</style>
