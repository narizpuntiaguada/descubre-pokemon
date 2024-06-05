<template>
  <div id="app" class="container col-12">
    <div> </div>
    <header>
      <div>
     
        <h1 class="">Pokemón</h1>
        <h3 class="pokeTitle text-center">¿Quién es ese Pokémon?</h3>
        <h5 class="text-center fw-bold">
          Pokemones descubiertos: <span> {{ counter }}</span
          >/20
        </h5>
      </div>
    </header>
    <main>
      <div class="row">
        <div
          class="col-sm-5 col-lg-3"
          v-for="(pokemon, index) in pokemones"
          :key="index"
        >
          <CardPoke :pokemon="pokemon" @correctAnswer="increase" />
          <!--a la izquierda está el nombre de Props y la derecha el valor que se le entrega /data binding-->
        </div>
      </div>
    </main>
  </div>
</template>
<script>
import axios from "axios";
import CardPoke from "./components/CardPoke.vue";
export default {
  name: "App",
  components: {
    CardPoke,
  },
  data() {
    return {
      pokemones: [],
      counter: 0,
    };
  },
  methods: {
    async getPoke() {
      try {
        let response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon/?offset=80&limit=32"
        );
        console.log(response);
        this.pokemones = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    increase() {
      this.counter++;
    },
  },
  mounted() {
    this.getPoke();
  },
};
</script>
<style scoped>
#app {
  font-family: "Pokemon Solid", sans-serif;
  color: rgb(24, 25, 2);
}
h1{
  text-align: center;
  font-family: "Pokemon Solid", sans-serif;
  color: rgb(182, 144, 19);
  text-shadow: 7px 0px 0px rgb(37, 7, 144), 0px -1px 0px rgb(65, 13, 168);

}
</style>