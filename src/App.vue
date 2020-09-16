<template>
  <div>
    <input v-model="nombre" type="text" @keyup.enter="buscarPokemon" placeholder="Busca un pokemon"/>
    <!--boton llamará a método buscar-->
    <button type="button" class="btn" @click="buscarPokemon">Buscar Pokemon</button>
    <div>
      <!--generamos baiding en src con imágen-->
      <img :src="imagen" alt="" />
      <h2>{{ nombrePokemon }}</h2>
      <hr />
      <div>
        <h5>Movimientos</h5>
        <div class="form-group">
          <select class="form-control" name="movimientos" id="moves">
            <option v-for="(movimiento, i) in movimientos" :key="i" value="">
              {{ movimiento.move.name }}
            </option>
          </select>
        </div>
      </div>
    </div>
    <div>
      <h5>Habilidades</h5>
      <div class="form-group">
        <select class="form-control" name="movimientos" id="moves">
          <option v-for="(habilidad, i) in habilidades" :key="i" value="">
            {{ habilidad.ability.name }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  //ESTADO
  //data es un método que retorna un objeto
  data() {
    return {
      //pondremos variable que vamos a necesitar para input
      nombre: "",
      //variable pokemon que será un objeto vacío el que se sustituirá por la espuesta de la API
      pokemon: {
        name: "",
        sprites: { front_default: "" },
        moves: [],
        abilities: [],
      },
    };
  },
  //utilizamos hook created (que es un método), que procesa una lógica donde llamará a una API (llamarémos a un método y para acceder al modelo utilizamos this)
  created() {
    this.buscarPokemon();
  },
  //creamos el método buscar pokemon y consultarlo en la API donde le pasamos a fetch como argumento la URL de la API
  methods: {
    buscarPokemon() {
      fetch(this.url)
        .then(function (response) {
          return response.json();
        })
        .then((myJson) => {
          console.log(myJson);
          //reasignamos el valor a pokemon
          this.pokemon = myJson;
        });
    },
  },
  //transformando en computed properties las que siempre retornan (en este caso retornará el mapeo)
  computed: {
    imagen() {
      return this.pokemon.sprites.front_default;
    },
    nombrePokemon() {
      return this.pokemon.name;
    },
    movimientos() {
      return this.pokemon.moves;
    },
    habilidades() {
      return this.pokemon.abilities;
    },
    url() {
      return this.nombre == ""
        ? `${this.setUrl}pikachu`
        : `${this.setUrl}${this.nombre.toLowerCase()}`;
    },
    setUrl() {
      return "https://pokeapi.co/api/v2/pokemon/";
    },
  },
};
</script>

<style lang="scss">
.foto_pokemon {
  width: 40%;
}
.navbar {
  background-color: red;
}
.btn {
  margin-left: 40px;
  color: black;
  background-color: red;
}
h1 {
  margin: 20px 0px;
}
</style>

