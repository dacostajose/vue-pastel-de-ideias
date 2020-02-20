<template>
  <div class="home">
    <div id="waves">
      <img src="../assets/logo.svg" id="logo" />
    </div>
    <div id="div-imagem-pasteis">
      <img src="../assets/pasteis-img.png" />
    </div>

    <div id="basecontent">
      <GerarCardapio :method="addItem" />
    </div>
    <div id="div-imagem-pasteis-desfocado">
      <img src="../assets/pastel-paralax.png" />
    </div>
    <ProdutList v-bind:listItens="cardapio" />
  </div>
</template>

<style lang="scss">
.home {
  background-image: url("../assets/background.png");
}
#waves {
  width: 100%;
  min-height: 60vh;
  background-image: url("../assets/wave.svg");
  background-size: cover;
  z-index: -1;
  #logo {
    margin-top: 2.5rem;
    width: 40vw;
    max-width: 30rem;
    height: auto;
  }
}
#div-imagem-pasteis {
  margin-top: -17rem;
  display: flex;
  justify-content: right;
  width: 80%;
}

#basecontent {
  display: flex;
  justify-content: center;
  margin-top: -11rem;
  width: 100%;
}
#div-imagem-pasteis-desfocado {
  z-index: 20;
  position: absolute;
  top: 15%;
  left: -2%;
  margin: 0;
  padding: 0;
  img {
    width: 80%;
    height: auto;
  }
}
</style>

<script>
// @ is an alias to /src
import GerarCardapio from "../components/cardapio";
import ProdutList from "../components/prodList";
export default {
  name: "Home",
  components: {
    GerarCardapio,
    ProdutList
  },
  data: function() {
    return {
      cardapio: []
    };
  },
  methods: {
    addItem(item) {
      this.cardapio.unshift(item);
    }
  }
};
</script>
