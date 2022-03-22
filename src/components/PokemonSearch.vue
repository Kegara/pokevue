<template>
  <div class=" flex flex-row w-full flex justify-center my-5 searcher">
    <input v-model="searchvalue" type="search" placeholder="Search Pokemon" class=" py-1" />
    <button
      @click="searchPokemon"
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-4 rounded"
    >
      Search
    </button>
  </div>
</template>

<script>
export default {
  name: "PokemonSearch",
  props: {
    apiUrl: String,
    setPokemonUrl: Function
  },
  data: () => {
    return {
      searchvalue: ""
    };
  },
  methods: {
    searchPokemon() {
      if (this.searchvalue !== "") this.$emit("setPokemonUrl", this.apiUrl + this.searchvalue);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  width: 80%;
  border-radius: 4px;
  border: 3px solid #ccc;
  margin-right: 1em;
  border-right: 0;
  border-left: 0;
  border-top: 0;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
