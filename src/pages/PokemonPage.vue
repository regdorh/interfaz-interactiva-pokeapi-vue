<template>
    <div class="container" v-if="!gameover">
      <div class="contet">
        <h1 v-if="!pokem"><span class="loader"></span></h1>
  
        <div v-else class="conte">
          <div id="poke">
            <h1>¿Who is this pokemon?</h1>
          </div>
  
          <PokemonPictureVue :pokemonId="pokem.id" :ShowPokemon="showpokemn" />
  
          <PokemonOptionsVue :pokemons="pokemonArr" @selection="traerpokem" :seleccion="selection" />
  
          <div v-if="showasnwers">
            <h3 class="fade-in" style="color: black;">{{ message }}</h3>
            <button @click="newgame" v-if="fallas >= 0 && fallas < 10" class="newbutton">New Game</button>
          </div>
  
          <div v-if="mostrar">
            <button @click="resetGame">Restart</button>
          </div>
  
        </div>
  
        <div v-if="pokem" class="pokacertado">
          <PokemonesAsertados :pokems="pokemonesAcertados" :correctas="correctas" />
        </div>
  
        <div v-if="pokem" class="aconrts">
         
        </div>
      </div>
    </div>
  
  
    <div v-if="gameover ">
      <div class="game-over-overlay">
        <p class="game-over-text">GAME OVER 😔</p>
        <h1>Haz Acertado {{ correctas }} Pokemons</h1>
        <h2 v-if="correctas>=5">Eres Fanatico de Pokemons 😄 </h2>
        <button @click="resetGame" class="restart"> Restart</button>
      </div>
    </div>
  </template>
  
  <script>
  import PokemonPictureVue from '@/components/PokemonPicture'
  import PokemonOptionsVue from '@/components/PokemonOptions'
  import getPokemonOptions from '@/helpers/getPokemonOptions'
  import PokemonesAsertados from "@/components/PokemonesAsertados.vue";

  export default {
    name: 'PokemonPage',
    components: { PokemonOptionsVue, PokemonPictureVue, PokemonesAsertados },
  
    data() {
      return {
        pokemonArr: [],
        pokem: null,
        showpokemn: false,
        message: null,
        showasnwers: false,
        isLoading: true,
        fallas: 0,
        correctas: 0,
        mostrar: false,
        gameover: false,
        pokemonesAcertados: [],
        imgSrc: "",
        selection: false
  
      }
    },

    methods: {
      async mixPokemonArray() {
        this.isLoading = false;
        this.pokemonArr = await getPokemonOptions()
        this.isLoading = true;
  
        const randon = Math.floor(Math.random() * 4)
  
        this.pokem = this.pokemonArr[randon]
      },
  
      traerpokem(pokemonId) {
        const pokems = this.pokemonArr.find(p => p.id == pokemonId)
        const pokn = this.pokem
  
        if (pokems.id !== pokn.id) {
          this.message = ` ❌ oh!! you failed try again 😞`;
          this.fallas++
          if (this.fallas >= 10) {
            this.message = "GAME OVER";
            this.mostrar = true
            this.gameover = true;
          }
        }
        else {
          this.message = ` ✅ Yeah !!! it's right 😎 `
          this.correctas++

          this.pokemonesAcertados.push(pokn);
  
        }
  
        this.showpokemn = true
        this.showasnwers = true
        this.selection = true
      },
  
      updateImgSrc(imgSrc) {
        this.imgSrc = imgSrc;
      },
      newgame() {
  
        this.showpokemn = false
        this.showasnwers = false
        this.pokem = null
        this.selection = false
        this.mixPokemonArray()
  
      },
      resetGame() {
        this.showpokemn = false;
        this.showasnwers = false;
        this.pokem = null;
        this.fallas = 0;
        this.correctas = 0
        this.mostrar = false
        this.gameover = false
        this.pokemonesAcertados = []
        this.selection = false
        this.mixPokemonArray();
  
      }
    },
  
    mounted() {
      this.mixPokemonArray();
    }
  
  }
  </script>
  
  <style scoped>
  .conte {
    position: relative;
    border: 1px solid rgb(121, 116, 116);
    border-radius: 25px;
    box-shadow: 5px 5px 20px rgb(195, 202, 204);
    margin: 0 auto;
    width: 100%;
    overflow: hidden;
    max-width: 325px;
    padding: 10px;
    background: rgb(255, 253, 253);
    background: linear-gradient(90deg, rgba(255, 253, 253, 1) 0%, rgba(135, 130, 130, 0.9780287114845938) 35%, rgba(31, 31, 31, 0.9528186274509804) 100%);
  
  }
  
  
  button {
    margin-bottom: 15px;
  }
  
  .table {
    font-family: Arial, Helvetica, sans-serif;
    margin: 0 auto;
    justify-content: space-around;
    align-items: center;
    padding: 10px;
  
  }
  
  #custom {
    font-family: Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }
  
  table {
    border-collapse: collapse;
    width: 15%;
    font-size: 16px;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    background-color: rgb(107, 107, 93)
  }
  
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 90vh;
  
  }
  
  .contet {
    display: flex;
    justify-content: center;
    width: auto;
    margin-top: 30px;
  
  
  }
  
  #customers td,
  #customers th {
    border: 1px solid #ddd;
    padding: 8px;
  }
  
  #customers tr:nth-child(even) {
    background-color: #f2f2f2;
  }
  
  #customers tr:hover {
    background-color: #ddd;
  }
  
  #customers th {
    margin: 10px;
    padding: 5px;
  }
  
  .loader {
    position: absolute;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    display: inline-block;
    border-top: 3px solid #FFF;
    border-right: 3px solid transparent;
    box-sizing: border-box;
    animation: rotation 1s linear infinite;
  }
  
  .pokacertado {
    border: 1px solid rgb(121, 116, 116);
    border-radius: 30px;
    box-shadow: 0 10px 30px 10px rgb(100, 100, 98);
    margin: 0 auto;
    width: 100%;
    overflow: hidden;
    max-width: 325px;
    margin-left: 40px;
    padding: 10px;
    background: rgb(0, 0, 0);
    background: linear-gradient(90deg, rgba(0, 0, 0, 1) 0%, rgba(68, 68, 68, 0.9780287114845938) 35%, rgba(136, 136, 136, 0.9528186274509804) 100%);
  }
  
  .game-over-text {
    font-size: 24px;
    font-weight: bold;
    text-align: center;
    margin-bottom: 10px;
    text-align: center;
  }
  
  .game-over-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.7);
    z-index: 999;
  }
  
  .game-over-text {
    font-size: 48px;
    font-weight: bold;
    text-align: center;
    color: white;
    margin-bottom: 20px;
    animation: fadeIn 1s ease-in-out;
  }
  
  .game-over-text button {
    font-size: 24px;
    background-color: #3498db;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .game-over-text button:hover {
    background-color: #2980b9;
  }
  
  .newbutton {
    padding: 10px;
    border-radius: 5px;
    background-color: #a19494;
    transition: all 0.5s;
  }
  
  .newbutton:hover {
    cursor: pointer;
    color: white;
    background-color: #3f3e3e;
    padding-inline-end: 30px;
    padding-inline-start: 30px;
    transition: all 0.5s;
  }
  
  .restart {
    padding: 10px;
    border-radius: 5px;
    background-color: #817a7a;
    transition: all 0.4s;
  }
  
  .restart:hover {
    cursor: pointer;
    background-color: #3f3e3e;
    color: white;
    padding-inline-end: 30px;
    padding-inline-start: 30px;
    transition: all 0.4s;
  }
  
  @keyframes fadeIn {
    0% {
      opacity: 0;
    }
  
    100% {
      opacity: 1;
    }
  }
  
  @keyframes rotation {
    0% {
      transform: rotate(0deg);
    }
  
    100% {
      transform: rotate(360deg);
    }
  }
  
  @media screen and (max-width: 510px) {
    .contet {
      margin-top: 35px;
      flex-direction: column;
  
    }
  
    .container {
      height: 100%;
      width: 100%;
      display: flex;
    }
  
    .pokacertado {
      margin-top: 50px;
      justify-content: center;
      align-items: center;
      height: 50%;
      margin-left: 0px;
    }
    .aconrts{
      margin-top: 20px;
      margin-bottom: 20px;
      margin-right: 54px;
    }
    .aconrts p{
      color: #e2d9d9;
    }
  }
  
  .aconrts h1 {
    font-size: 2.5em;
    font-weight: 700;
    color: #b9c9da;
    margin: 0;
    padding: 0;
    text-align: center;
  
  }
  
  .aconrts {
    margin-left: 50px;
    display: flex;
    justify-content: center;
    align-items: start;
    width: auto;
  }
  
  .aconrts li {
    list-style: none;
    width: 20px;
    height: 23px;
    font-size: 23px;
    border-radius: 20px;
  }
  
  .aconrts li,
  p {
    margin-top: 0px;
  
  }
  
  .aconrts p {
    color: #ffffff;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
  
  }
  
  .aconrts p:hover {
    color: #b4b3b3;
  
  
  }</style>