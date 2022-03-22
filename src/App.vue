<template>
  <div class="flex items-center flex-col my-10 bg-pokeball px-2">
    <h1 class="text-4xl basis-full w-fit mt-10 pokefont">POKEVUE
    </h1>
    <PokemonList :imageUrl="imageUrl" :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
  </div>
</template>

<script>
import PokemonList from "@/components/PokemonList.vue";

export default {
  data: () => {
    return {
      imageUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
      pokemonUrl: "https://pokeapi.co/api/v2/pokemon/1",
      showDetail: false
    };
  },
  components: {
    PokemonList
  }
};
</script>
<style >
@font-face {
  font-family: "pokemon_hollownormal";
  src: url("../public/font/pokemonfont-webfont.woff2") format("woff2"),
    url("../public/font/pokemonfont-webfont.woff") format("woff");
}
.bg-pokeball {
  background-image: url("../public/img/pokeball-image-background.png");
  background-position: center;
  background-repeat: repeat;
  margin: 0 auto;
}
* {
  font-family: "Flexo-Regular";
}
.pokefont {
  font-family: "pokemon_hollownormal";
  color: blue;
  letter-spacing: 6pt;  
  -webkit-text-stroke-width: 1.3px;
  text-shadow: 0 0 10px yellow,
                 0 0 10px yellow,
                 0 0 10px yellow,
                 0 0 5px yellow,
                 0 0 5px yellow,
                 0 0 5px yellow;

}
</style>
