<template>
  <div id="produtList">
    <div v-if="listItens.length > 0" id="line-header-container">
      <div class="line" />
      <span>Veja como será apresentado ao cliente</span>
      <div class="line" />
    </div>
    <div id="cardapio-item-container" v-for="(item, index) in listItens" v-bind:key="index">
      <div class="cardapio-item">
        <div class="cardapio-item-header">
          <span class="cardapio-item-title">{{ item.foodTitle }}</span>
          <span class="cardapio-item-price">
            {{
            new Intl.NumberFormat("pt-Br", {
            style: "currency",
            currency: "BRL"
            }).format(item.foodPrice)
            }}
          </span>
        </div>
        <div class="cardapio-item-content">
          <img v-if="item.imageData" v-bind:src="item.imageData" />
          <img v-else-if="item.isDrimk" src="../assets/bebida-padrao.jpeg" />
          <img v-else src="../assets/pastel-img.png" />

          <div class="cardapio-item-content-text">
            <p>
              <strong>Sabor:</strong>
              {{ item.foodFlavor }}
            </p>
            <p>
              <strong>Descrição:</strong>
              {{ item.foodDescription }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProdutList",
  props: {
    listItens: { type: Array }
  },
  data: function() {
    return {};
  },
  methods: {}
};
</script>

<style scoped lang="scss">
#produtList {
  margin-top: 5rem;
}
#line-header-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  .line {
    flex-grow: 1;
    border: 1px solid #e43636;
    height: 0px;
  }
  span {
    margin-left: 5%;
    margin-right: 5%;
    color: #a03400;
  }
}
#cardapio-item-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-items: center;
}
.cardapio-item {
  background-color: #fff;
  width: 50%;
  border-radius: 20px;
  box-shadow: 0px 0px 30px #740b0b45;
  padding-bottom: 1rem;
  margin: 0;
  padding: 0;
  box-shadow: 0px 0px 30px #740b0b45;
  margin-top: 1rem;
}
.cardapio-item-header {
  flex-grow: 1;
  background-color: #e43636;
  border-radius: 20px 20px 0px 0px;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 1rem 1rem 1rem 4rem;
  font-size: 1.25rem;
  font-style: italic;
  .cardapio-item-title {
    color: #ffca00;
    flex-grow: 1;
    text-align: left;
    &::after,
    &::before {
      content: '"';
    }
  }
  .cardapio-item-price {
    color: #fff;
  }
}
.cardapio-item-content {
  border-radius: 0px 0px 20px 20px;
  padding: 1rem 1rem 1rem 4rem;
  display: flex;
  flex-direction: row;
  img {
    width: 180px;
    height: 180px;
    margin-left: -12rem;
    margin-top: -3rem;
    z-index: 1;
    border-radius: 20px;
    box-shadow: 0px 0px 30px #740b0b45;
  }
}
.cardapio-item-content-text {
  text-align: left;
  color: #a03400;
  z-index: 2;
  margin-left: 1rem;
}
</style>
