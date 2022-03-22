<template>
  <div class="flex flex-col container">
    <pokemon-search @setPokemonUrl="setPokemonUrl" :apiUrl="apiUrl" />
    <pokemon-detail :pokemonUrl="this.pokeUrl" class="" />
    <div class="w-full">
      <h1 class="text-4xl pokefont">Pokemon List</h1>
      <div class="list">
        <a
          v-for="(pokemon, index) in pokemons"
          :key="'poke' + index"
          @click="setPokemonUrl(pokemon.url)"
          href="#detail"
          :class="[
            pokemon?.types[0]['type'].name,
            'flex pokarticle',
            'justify-center',
            'items-center',
            'rounded',
            'cursor-pointer'
          ]"
        >
          <div class="row-auto flex justify-around items-center text-center">
            <h3 class="text-white text-3xl font-bold">{{ pokemon.name }}</h3>
            <img :src="imageUrl + pokemon.id + '.png'" alt="" />
          </div>
        </a>
        <div id="scroll-trigger" ref="infinitescrolltrigger">
          <i class="fas fa-spinner fa-spin"></i>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import PokemonDetail from "@/components/PokemonDetail.vue";
import PokemonSearch from "@/components/PokemonSearch.vue";

export default {
  props: ["imageUrl", "apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
      pokeUrl: "https://pokeapi.co/api/v2/pokemon/3/"
    };
  },
  components: {
    PokemonDetail,
    PokemonSearch
  },
  methods: {
    async fetchData() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then(resp => {
          if (resp.status === 200) return resp.json();
        })
        .then(data => {
          this.nextUrl = data.next;
          data.results.forEach(pokemon => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function(part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .then(() => {
          this.pokemons.forEach(pokemon => {
            let req = new Request(pokemon.url);
            fetch(req)
              .then(resp => {
                if (resp.status === 200) return resp.json();
              })
              .then(data => {
                pokemon.types = data.types;
              })
              .catch(error => {
                console.log(error + "error color");
                pokemon.types = null;
              });
          });
        })
        .catch(error => {
          console.log(error + "error list");
        });
    },
    scrollTrigger() {
      const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonUrl(url) {
      this.pokeUrl = url;
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  }
};
</script>

<style>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-gap: 10px;
  width: 100%;
}
.pokarticle {
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  min-height: 15vh;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}
.normal {
  background-color: #a8a878;
  border-color: #6d6d4e;
}
.fire {
  background-color: #f08030;
  border-color: #9c531f;
}
.water {
  background-color: #6890f0;
  border-color: #445e9c;
}
.electric {
  background-color: #f8d030;
  border-color: #a1871f;
}
.grass {
  background-color: #78c850;
  border-color: #4e8234;
}
.ice {
  background-color: #98d8d8;
  border-color: #638d8d;
}
.fighting {
  background-color: #c03028;
  border-color: #7d1f1a;
}
.poison {
  background-color: #a040a0;
  border-color: #682a68;
}
.ground {
  background-color: #e0c068;
  border-color: #927d44;
}
.flying {
  background-color: #a890f0;
  border-color: #6d5e9c;
}
.psychic {
  background-color: #f85888;
  border-color: #a13959;
}
.bug {
  background-color: #a8b820;
  border-color: #6d7815;
}
.rock {
  background-color: #b8a038;
  border-color: #786824;
}
.ghost {
  background-color: #705898;
  border-color: #493963;
}
.dragon {
  background-color: #7038f8;
  border-color: #4924a1;
}
.dark {
  background-color: #705848;
  border-color: #49392f;
}
.steel {
  background-color: #b8b8d0;
  border-color: #787887;
}
.fairy {
  background-color: #ee99ac;
  border-color: #9b6470;
}
</style>
