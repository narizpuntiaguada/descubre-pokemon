<template>
  <div id="CardPoke" class="text-center my-3">
    <div class="card" style="">
      <!-- Hicimos biding de clase :) -->
      <img
        :src="imagenPokemon"
        class="card-img-top my-2"
        alt="logo pokemon"
        :class="esconderPokemon ? 'borroso' : ''"
      />
      <div class="card-body">
        <p class="paragraphName text-center" v-show="!esconderPokemon">
          {{ pokemon.name }}
        </p>
        <form v-show="esconderPokemon">
          <input
            type="text"
            class="form-control my-2"
            placeholder="Escribe el nombre aquí"
            v-model="descubrirPokemon"
            @keyup.enter="descubrir"
          />
          <div class="d-grid my-2">
            <button
              @click.prevent="descubrir"
              class="btn btn-primary"
              :style="{ backgroundColor: button }"
            >
              Descubrir
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CardPoke",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      infoPokemon: {},
      esconderPokemon: true,
      descubrirPokemon: "",
    };
  },
  computed: {
    imagenPokemon() {
      return this.infoPokemon.sprites?.other["official-artwork"].front_default;
    },
  },
  // Ciclo de vida de los componentes (ejecuta lo que esta dentro de la función created)
  created() {
    this.getInfoPokemon();
  },
  methods: {
    async getInfoPokemon() {
      try {
        //  destructurar {data}
        let { data } = await axios.get(this.pokemon.url);
        this.infoPokemon = data;
      } catch (error) {
        console.log(error);
      }
    },
    descubrir() {
      if (
        this.descubrirPokemon.toLowerCase() === this.pokemon.name.toLowerCase()
      ) {
        this.esconderPokemon = false;
        this.$emit("correctAnswer");
      } else {
        console.log("incorrecto");
        this.$swal.fire({
          title: "Ups!",
          text: "Intenta nuevamente",
          icon: "error",
          confirmButtonText: "Continuar",
        });
      }
    },
  },
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  font-family: "Gruppo", sans-serif;
  background-color: rgba(162, 231, 147, 0.767);
  border-radius: 10%;
}
.borroso {
  filter: blur(5px) grayscale(100%);
}

img {
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>