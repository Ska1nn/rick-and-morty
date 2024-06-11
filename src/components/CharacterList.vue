<template>
    <div>
      <div>
        <input v-model="filters.name" placeholder="Имя" />
        <select v-model="filters.status">
          <option value="">Любой Статус</option>
          <option value="alive">Живой</option>
          <option value="dead">Мертвые</option>
          <option value="unknown">Неизвестно</option>
        </select>
        <button class="print-search" @click="applyFilters">Применить</button>
      </div>
  
      <div class="vs-info" v-if="characters.length">
        <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
      </div>
      <div v-else>
        Персонажи не найдены.
      </div>
  
      <div class="arrow-6">
        <button class="pagi-prev" @click="changePage(-1)" :disabled="page === 1">
          <svg width="18px" height="17px" viewBox="0 0 18 17" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
        <g transform="translate(8.500000, 8.500000) scale(-1, 1) translate(-8.500000, -8.500000)">
            <polygon class="arrow-6-pl" points="16.3746667 8.33860465 7.76133333 15.3067621 6.904 14.3175671 14.2906667 8.34246869 6.908 2.42790698 7.76 1.43613596"></polygon>
            <polygon class="arrow-6-pl-fixed" points="16.3746667 8.33860465 7.76133333 15.3067621 6.904 14.3175671 14.2906667 8.34246869 6.908 2.42790698 7.76 1.43613596"></polygon>
            <path d="M-1.48029737e-15,0.56157424 L-1.48029737e-15,16.1929159 L9.708,8.33860465 L-2.66453526e-15,0.56157424 L-1.48029737e-15,0.56157424 Z M1.33333333,3.30246869 L7.62533333,8.34246869 L1.33333333,13.4327013 L1.33333333,3.30246869 L1.33333333,3.30246869 Z"></path>
        </g>
    </svg></button>
        <button class="pagi-next" @click="changePage(1)">
          <svg width="18px" height="17px" viewBox="-1 0 18 17" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
        <g>
            <polygon class="arrow-6-pl" points="16.3746667 8.33860465 7.76133333 15.3067621 6.904 14.3175671 14.2906667 8.34246869 6.908 2.42790698 7.76 1.43613596"></polygon>
            <polygon class="arrow-6-pl-fixed" points="16.3746667 8.33860465 7.76133333 15.3067621 6.904 14.3175671 14.2906667 8.34246869 6.908 2.42790698 7.76 1.43613596"></polygon>
            <path d="M-4.58892184e-16,0.56157424 L-4.58892184e-16,16.1929159 L9.708,8.33860465 L-1.64313008e-15,0.56157424 L-4.58892184e-16,0.56157424 Z M1.33333333,3.30246869 L7.62533333,8.34246869 L1.33333333,13.4327013 L1.33333333,3.30246869 L1.33333333,3.30246869 Z"></path>
        </g>
    </svg>
  </button>
      </div>
    </div>
  </template>
  <style>
  input {
    background: #ffffff;
    padding: 6px 10px;
    border: 1px solid #272b33;
}
select {
    background: #ffffff;
    padding: 5px 8px;
    border: 10px solid #272b33;
}
button.print-search {
    background: #ff9800;
    color: #ffffff;
    border: 1px solid #272b33;
    padding: 6px 10px;
    transition: background-color 0.3s;
}
button.print-search:hover {
    background: #ffffff;
    color: #000000;
}
  .vs-info {
    /* background: aqua; */
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    align-content: stretch;
}
  .arrow-6 {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    align-content: stretch;
}
    .click-pagination {
    background: antiquewhite;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: stretch;
}
button.pagi-prev {
    background: #ff9800;
    border: none;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
}
button.pagi-next {
    background: #ff9800;
    border: none;
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
}
    .arrow-6 {
    margin:20px;
}
.arrow-6 svg {
    width: 20px;
    height: auto;
    margin: 0 2rem;
    cursor: pointer;
    overflow: visible;
}
.arrow-6 svg polygon, 
.arrow-6 svg path {
    transition: all 0.5s cubic-bezier(0.2, 1, 0.3, 1);
    fill: #ffffff;
}
.arrow-6 svg:hover polygon, 
.arrow-6 svg:hover path {
    transition: all 1s cubic-bezier(0.2, 1, 0.3, 1);
    fill: #000;
}
.arrow-6 svg:hover .arrow-6-pl {
    animation: arrow-6-anim 1s cubic-bezier(0.2, 1, 0.3, 1) infinite;
}
.arrow-6 svg:hover .arrow-6-pl-fixed {
    animation: arrow-6-fixed-anim 1s cubic-bezier(0.2, 1, 0.3, 1) infinite;
}
 
@keyframes arrow-6-anim {
    0% {
        opacity: 1;
        transform: translateX(0);
    }
    5% {
        transform: translateX(-0.1rem);
    }
    100% {
        transform: translateX(1rem);
        opacity: 0;
    }
}
@keyframes arrow-6-fixed-anim {
    5% {
        opacity: 0;
    }
    20% {
        opacity: 0.4;
    }
    100% {
        opacity: 1;
    }
}
  </style>
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
  