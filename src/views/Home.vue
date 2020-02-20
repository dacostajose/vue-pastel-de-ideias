<template>
  <div class="home">
    <div id="waves">
      <img src="../assets/logo.svg" id="logo" />
    </div>
    <div id="basecontent">
      <GerarCardapio :method="addItem" />
    </div>
    <ProdutList v-bind:listItens="cardapio" />
  </div>
</template>

<style lang="scss">
#waves {
  width: 100%;
  min-height: 60vh;
  background-image: url("../assets/wave.svg");
  background-size: cover;
  z-index: -1;
  #logo {
    margin-top: 2.5rem;
    width: 30rem;
    height: auto;
  }
}

#basecontent {
  display: flex;
  justify-content: center;
  margin-top: -15rem;
  width: 100%;
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
