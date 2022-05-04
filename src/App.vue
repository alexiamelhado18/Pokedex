<template>
  <div id="app">
    <main>
      <input id="campo-pesquisar" type="text" placeholder="Search" v-model="namePokemon" @input="filter(namePokemon)">
      <section>
        <div class="section" v-if="processed">
          <ComponentPokemon v-for="(obj, i) in pokemons" :key="obj.id" v-bind:number="'#' + (i + 1)"
            v-bind:name="obj.name" v-bind:image="obj.sprites.front_default" />

        </div>
        <div class="section" v-else>
          <ComponentPokemon v-bind:number="'#' + pokemon.id"
            v-bind:name="pokemon.name" v-bind:image="pokemon.sprites.front_default" />

        </div>
        
      </section>
    </main>
  </div>
</template>

<script>
import ComponentPokemon from './components/ComponentPokemon.vue'

export default {
  name: 'App',
  components: {
    ComponentPokemon
  },
  data: () => ({
    pokemons: [],
    namePokemon: '',
    pokemon: {},
    processed: true,

  }),
  async created() {

    for (let i = 1; i < 152; i++) {
      fetch("https://pokeapi.co/api/v2/pokemon/" + i)
        .then(async response => {
          const data = await response.json();
          this.pokemons.push(data);
          // console.log(this.pokemons);
        }).catch(() => {
          console.error("There was an error!");
        });
    }
  },
  methods: {
    filter(name) {
      // console.log(name);
      if (name.length >= 3) {
        fetch("https://pokeapi.co/api/v2/pokemon/" + name.toLowerCase())
          .then(async response => {
            const data = await response.json();
            this.pokemon = data;
            console.log(this.pokemon);
            this.processed = false;
          })
          .catch(err => {
            console.log(err);
          });

      }
      if (name == '') {
        document.location.reload(true)
      }
    }
  }
}
</script>

<style lang="less">
@bottom-gray: #ededed;
@color-medium-gray: #a9a9a9;

main {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  background: @bottom-gray;
  padding: 0 15px;
}

#campo-pesquisar {
  outline: none;
  margin: 20px 0;
  width: 100%;
  border-top: 0;
  border-left: 0;
  border-right: 0;
  border-bottom: 2px solid @color-medium-gray;
  background: @bottom-gray;
  color: @color-medium-gray;
}
section{width: 100%;}
.section{
  width: 100%;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  column-gap: 10px;
  row-gap: 10px;
}

@media only screen and (max-width: 400px) {
  .section{
    grid-template-columns: 1fr;
  }
}

@media only screen and (min-width: 401px) {
  .section{
    grid-template-columns: repeat(2, 1fr);

  }
}

@media only screen and (min-width: 630px) {
  .section{
    grid-template-columns: repeat(3, 1fr);

  }
}

@media only screen and (min-width: 900px) {
  .section{
    grid-template-columns: repeat(4, 1fr);

  }
}

@media only screen and (min-width: 1257px) {
  .section{
    grid-template-columns: repeat(5, 1fr);

  }
}

@media only screen and (min-width: 1500px) {
  .section{
    grid-template-columns: repeat(6, 1fr);

  }
}
</style>

