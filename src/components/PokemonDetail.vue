<template>
  <div :class="[pokeColor, 'flex justify-around items-center py-4 flex-col md:!flex-row']" id="detail">
    <div :class="[pokeColor, 'py-4 w-80 text-white font-bold']">
      <p class="text-4xl capitalize mx-2">{{ pokemon.name }}</p>
      <p class="text-right text-lg mx-2">#{{ pokemon.id }}</p>
      <br />
      <span class="tag" v-for="(type, index) in pokemon.types" :key="'type' + index">
        {{ type.type.name }}
      </span>
      <div
        class="
          align-middle
          justify-center
          items-center
          flex
          self-center
          justify-self-center
          vh-20
        "
      >
        <img :src="pokeImage" alt="" class="inline w-full" style="z-index: index 3" />
      </div>
    </div>
    <div class="nav-tags flex flex-col align-middle justify-around py-4 m-4">
      <div class="flex align-middle justify-around">
        <button v-on:click="toggleNav()">Stadistics</button>
        <button v-on:click="toggleNav()">Movements</button>
      </div>
     
      <div v-if="opc">
        <div
          class="
            grid grid-cols-5
            gap-y-2
            align-middle
            justify-around
            mx-6
            py-4
            items-center
          "
          v-for="(stats, index) in pokemon.stats"
          :key="'stat' + index"
        >
          <p class="capitalize">{{ stats.stat.name }}</p>
          <p class="text-center">{{ stats.base_stat }}</p>
          <div
            class="
              w-full
              col-span-3
              bg-gray-200
              rounded-full
              h-2.5
              dark:bg-gray-700
            "
          >
            <div
              :class="[pokeColor,'h-2.5 rounded-full']"
              v-bind:style="{ width: stats.base_stat > 100 ? 100 : stats.base_stat + '%' }"
            ></div>
          </div>
        </div>
      </div>
       <div v-if="!opc" class="
            max-h-40">
        <div
          class="
            grid grid-cols-5
            gap-12
            align-middle
            justify-around
            mx-6
            py-4
            items-center
           
          "
        
        >
          <p class="capitalize "   v-for="(item, index) in pokemon.moves"
          :key="'move' + index">{{item.move.name}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl"],
  watch: {
    pokemonUrl(newUrl) {
      this.pokeUrl = newUrl;
      this.fetchData();
    }
  },
  data: () => {
    return {
      opc: true,
      show: false,
      pokemon: []
    };
  },
  methods: {
    async fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then(resp => {
          if (resp.status === 200) return resp.json();
        })
        .then(data => {
          this.pokemon = data;
          this.show = true;
        })
        .catch(error => {
          console.log(error);
        });
    },
    toggleNav() {
      this.opc = !this.opc;
    }
  },
  created() {
    this.fetchData();
  },
  computed: {
    pokeColor() {
      try {
        return this.pokemon?.types[0]["type"].name;
      } catch (error) {
        return "";
      }
    },
    pokeImage() {
      try {
        return this.pokemon.sprites.front_default;
      } catch (error) {
        return error;
      }
    }
  }
};
</script>

<style>
.vh-20 {
  min-height: 20vh;
}

.tag {
  color: #fff;
  background-color: #ffffff73;
  border-radius: 20px;
  padding: 5px;
  margin: 5px;
}
.nav-tags {
  top: -15px;
  position: relative;
  background-color: #fff;
  z-index: 1;
  border-radius: 50px;
}
.nav-tags button {
  border: 2px solid rgb(248, 4, 4);
  border-top: 0;
  border-left: 0;
  border-right: 0;
}
.content {
  border-radius: 0 0 50px 50px;
}
.max-h-40{
  max-height: 40vh;
  overflow: scroll;
}
</style>
