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
import axios from "axios";
const api_url = "http://localhost:5000/";
const headRequest = {
  headers: {
    "Access-Control-Allow-Origin": "*"
  }
};
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
  mounted() {
    axios({
      method: "GET",
      url: api_url + "products",
      headRequest
    })
      .then(response => {
        this.cardapio = [...response.data.products];
      })
      .catch(e => console.log(e));
  },
  methods: {
    addItem(item, file) {
      axios({
        method: "POST",
        url: api_url + "product",
        data: item,
        headRequest
      })
        .then(response => {
          console.log(response);
          console.log(file);
          if (file) {
            let formData = new FormData();
            formData.append("image", file);
            axios({
              method: "PUT",
              url: api_url + "product/" + response.data.id,
              data: formData,
              headRequest
            }).then(response);
          }
        })
        .catch(e => console.log(e));
    }
  }
};
</script>
