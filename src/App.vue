<template>
  <div>
    <header class="header">
      <div class="hea">
        <p style="font-size: 40px;">Ingrese nombre o id</p><br>
        <img src="./assets/fuego.jpg" class="logo"><br>
        <input type="text" v-model="id"><br>
        <button @click="traer()" class="pokeb"></button>
      </div>
    </header>
    
    

    <div v-if="filteredTypes.length > 0" class="card1">
      <div class="pdr" v-for="(typeData, index) in filteredTypes" :key="index"
      :style="{ backgroundColor: tipoFondos[typeData.type.name] }">
  
        <div class="cont1">
          <h1>Datos:</h1>
          <h2 class="stat"> Nombre: <span style="color: aliceblue;">{{ pokemon.name }}</span> </h2>
          <h2 class="stat"> Peso: <span style="color: aliceblue;">{{ pokemon.weight/10 }} kg</span> </h2>
          <h2 class="stat"> Id: <span style="color: aliceblue;">{{ pokemon.id }}</span> </h2>
          <h2 class="stat"> Altura: <span style="color: aliceblue;">{{ pokemon.height/10 }} m</span> </h2>
        </div>
  
        <div class="card2">
  
          <div class="imagen_pokemon" >
            <img v-if="pokemon.sprites && pokemon.sprites.front_default" :src="pokemon.sprites.front_default" >
          </div>
          <h3 class="stat">Tipo:</h3>
          <p class="tip">
            <span v-for="(typeData, index) in pokemon.types" :key="index"
              :style="{ backgroundColor: tipoColores[typeData.type.name] }" class="stat">
              {{ traducirTipo(typeData.type.name) }}
              <span v-if="index !== pokemon.types.length - 1"><br></span>
            </span>
          </p>
        </div>
  
        <div class="cont2">
            <h1>Estadisticas:</h1>
            <li v-for="(stat, key) in pokemon.stats" :key="key">
            <progress :value="stat.base_stat" max="255"></progress> <span class="stat">{{ stat.stat.name }}:</span> <span class="stat">{{ stat.base_stat }}</span>   
            </li>
        </div>
  
      </div>
    </div>
    
      <div class="linea-superior"></div>
      <div class="linea-inferior"></div>
      <div class="circulo-blanco"></div>
      <div class="circulo-rojo"></div>
    
      
  </div>
</template>

<script setup>
import { ref,computed } from "vue"
import axios from "axios"


const filteredTypes = computed(() => {
  // Filtramos la lista para obtener solo el primer tipo
  return pokemon.value?.types?.slice(0, 1) || [];
});

let pokemon = ref([])



const tipoFondos = {
  fire: '#E72324',
  water: '#2481F0',
  grass: '#3DA224',
  normal: '#A0A2A0',
  fighting: '#FF8100',
  flying: '#82BAF0',
  poison: '#923FCC',
  ground: '#92501B',
  rock:  '#B0AB82',
  bug:  '#92A212',
  ghost: '#713F71',
  steel:  '#60A2B9',
  electric: '#FAC100',
  psychic: '#EF3F7A',
  ice: '#3DD9FF',
  dragon: '#4F60E2',
  dark: '#4F3F3D',
  fairy: '#EF71F0',
}


const tipoColores = {
  fire: '#E72324',
  water: '#2481F0',
  grass: '#3DA224',
  normal: '#A0A2A0',
  fighting: '#FF8100',
  flying: '#82BAF0',
  poison: '#923FCC',
  ground: '#92501B',
  rock:  '#B0AB82',
  bug:  '#92A212',
  ghost: '#713F71',
  steel:  '#60A2B9',
  electric: '#FAC100',
  psychic: '#EF3F7A',
  ice: '#3DD9FF',
  dragon: '#4F60E2',
  dark: '#4F3F3D',
  fairy: '#EF71F0',
};

let id = ref("")


async function traer() {
  try {
    let r = await axios.get("https://pokeapi.co/api/v2/pokemon/" + id.value)
    pokemon.value = r.data
    console.log(r.data);
    console.log(id.value);
  } catch (error) {
    console.log(error);
  }
}







const traduccionesTipos = {
  "normal": "Normal",
  "fighting": "Lucha",
  "flying": "Volador",
  "poison": "Veneno",
  "ground": "Tierra",
  "rock": "Roca",
  "bug": "Bicho",
  "ghost": "Fantasma",
  "steel": "Acero",
  "fire": "Fuego",
  "water": "Agua",
  "grass": "Planta",
  "electric": "Eléctrico",
  "psychic": "Psíquico",
  "ice": "Hielo",
  "dragon": "Dragón",
  "dark": "Siniestro",
  "fairy": "Hada"
};

function traducirTipo(tipo) {
  return traduccionesTipos[tipo] || tipo;
}
</script>
