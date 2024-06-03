<script>
export default {
  name: 'CharacterCard',
  props: {
    character: Object
  }
}
</script>
<template>
    <div class="card">
      <img :src="character.image" alt="Character Image" class="character-image"/>
      <h3>{{ character.name }}</h3>
      <p>Status: {{ character.status }}</p>
      <p>Species: {{ character.species }}</p>
      <p>Gender: {{ character.gender }}</p>
    </div>
  </template>
  <style>
  .card {
    border: 1px solid #ccc;
    padding: 16px;
    margin: 8px;
    width: 200px;
    text-align: center;
  }
  .character-image {
    width: 100%;
    height: auto;
    border-radius: 8px;
    margin-bottom: 8px;
  }
  </style>
  