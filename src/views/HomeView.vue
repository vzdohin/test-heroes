<script>
import axios from 'axios';
import CharacterCard from '../components/CharacterCard.vue';
import PaginationComponent from '../components/PaginationComponent.vue';

export default {
  components: {
    CharacterCard,
    PaginationComponent
  },
  props: {
    page: {
      type: Number,
      default: 1
    },
    name: {
      type: String,
      default: ''
    },
    status: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      characters: [],
      currentPage: this.page,
      hasNextPage: true,
      filters: {
        name: this.name,
        status: this.status
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
        this.updateRoute();
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
    },
    goHome() {
      this.filters.name = '';
      this.filters.status = '';
      this.fetchCharacters(1);
      this.$router.replace({ name: 'Home', query: { page: 1, name: '', status: '' } });
    },
    updateRoute() {
      this.$router.replace({
        name: 'Home',
        query: {
          page: this.currentPage,
          name: this.filters.name,
          status: this.filters.status
        }
      });
    }
  },
  mounted() {
    this.fetchCharacters(this.currentPage);
  }
}
</script>
<template>
  <div>
    <h1 @click="goHome" class="clickable-header">Rick and Morty Characters</h1>
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
  justify-content: center;
}

.clickable-header {
  display: inline-block;
  cursor: pointer;
  transition: opacity 0.3s ease;
}

.clickable-header:hover {
  opacity: 0.7;
}
</style>
