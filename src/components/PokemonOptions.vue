<template>
    <div class="options-container">
      <ul> 
        <li v-for="pokemn in pokemons" :key="pokemn.id" @click="$emit('selection', pokemn.id)" :class="{ 'disabled' : seleccion }" >
          {{ pokemn.name }}
        </li>
  
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemons: {
        type: Array,
        required: true
  
  
      },
      seleccion: {
        type: Boolean,
        required: true
        
      }
    },
    
    
  
  
  }
  </script>
  
  <style Scoped>
  ul {
    list-style-type: none;
    padding-right: 35px;
    width: 500px;
  }
  
  li {
    background-color: rgb(65, 64, 64);
    border-radius: 5px;
    border: 1px solid rgba(24, 23, 23, 0.2);
    cursor: pointer;
    margin-bottom: 10px;
    width: 100%;
    color: aliceblue;
  
  }
  
  li:hover {
    background-color: rgb(0, 0, 0)
  }
  
  .options-container {
    display: flex;
    justify-content: center;
    align-items: center;
  
  }
  
  .disabled {
    pointer-events: none;
    opacity: 0.5;
  }
  </style>