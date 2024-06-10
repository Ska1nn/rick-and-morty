<template>
    <div>
      <div>
        <input v-model="filters.name" placeholder="Name" />
        <select v-model="filters.status">
          <option value="">Any Status</option>
          <option value="alive">Alive</option>
          <option value="dead">Dead</option>
          <option value="unknown">Unknown</option>
        </select>
        <button @click="applyFilters">Apply</button>
      </div>
  
      <div v-if="characters.length">
        <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
      </div>
      <div v-else>
        No characters found.
      </div>
  
      <div>
        <button @click="changePage(-1)" :disabled="page === 1">Previous</button>
        <button @click="changePage(1)">Next</button>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import CharacterCard from './CharacterCard.vue';
  
  export default {
    name: "CharacterList",
    components: {
      CharacterCard
    },
    data() {
      return {
        characters: [],
        page: 1,
        filters: {
          name: '',
          status: ''
        }
      };
    },
    watch: {
      page: 'fetchCharacters',
    },
    methods: {
      fetchCharacters() {
        const params = {
          page: this.page,
          name: this.filters.name,
          status: this.filters.status
        };
  
        axios.get('https://rickandmortyapi.com/api/character', { params })
          .then(response => {
            this.characters = response.data.results;
          })
          .catch(error => {
            console.error(error);
          });
      },
      changePage(direction) {
        this.page += direction;
      },
      applyFilters() {
        this.page = 1;
        this.fetchCharacters();
      }
    },
    created() {
      this.fetchCharacters();
    }
  };
  </script>
  