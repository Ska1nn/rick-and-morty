<template>
    <div class="character-card">
      <img :src="character.image" alt="character image" />
      <h2>{{ character.name }}</h2>
      <p>Status: {{ character.status }}</p>
      <p>Species: {{ character.species }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "CharacterCard",
    props: {
      character: Object
    }
  };
  </script>
  
  <style scoped>
  .character-card {
    border: 1px solid #ccc;
    padding: 16px;
    margin: 16px;
    text-align: center;
  }
  img {
    max-width: 100%;
    height: auto;
  }
  </style>
  