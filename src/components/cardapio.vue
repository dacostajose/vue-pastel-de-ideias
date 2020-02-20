<template>
  <div id="cardapiobox">
    <div id="header">
      <h3>Monte aqui o seu cardápio. O que está esperando?</h3>
      <div>
        <span>Comida</span>
        <toggle-button color="#E33535" id="toggleButton" v-model="isDrimk" />
        <span>Bebida</span>
      </div>
    </div>
    <div id="contentInputs">
      <div id="row-inputs">
        <div class="mediuminput">
          <input
            v-model="foodTitle"
            placeholder="Titulo do pedido"
            class="inputPd mediuminput"
          />
          <span v-if="errofoodTitle"
            >O campo de titulo deve ter entre 3 e 60 caracteres*</span
          >
          <span v-else> </span>
        </div>
        <div class="mediuminput">
          <input
            required
            v-model="foodFlavor"
            placeholder="Sabor"
            class="inputPd mediuminput"
          />
          <span v-if="errofoodFlavor">
            O campo de sabor deve ter entre 3 e 60 caracteres*</span
          >
          <span v-else> </span>
        </div>
        <div class="smallInput">
          <input
            v-model.number="foodPrice"
            placeholder="Preço"
            type="number"
            class="inputPd smallInput"
            min="0"
          />
          <span v-if="errofoodPrice">O campo de valor não pode ser vazio*</span>
          <span v-else> </span>
        </div>
      </div>

      <textarea
        v-model="foodDescription"
        placeholder="Descrição"
        class="inputPd mediuminput"
      />

      <!-- coloquei para aceitar apenas imagens, pq o filtro nativo (accept="image/x-png, image/jpeg"), não funciona no firefox -->
      <input
        type="file"
        name="imagepicker"
        accept="image/*"
        @change="processFile($event)"
        id="imagepicker"
      />
      <label for="imagepicker" id="file-input-pd">
        <!-- <img src=""/> -->
        <img
          v-if="imageData"
          v-bind:src="imageData"
          style="width:10rem; height:auto"
        />
        <p v-if="!imageData">
          Jogue aqui o arquivo de imagem do seu pastel ou clique para localizar
          a pasta.
        </p>
        <p v-else>
          Clique aqui para alterar a imagem
        </p>
      </label>
      <div v-if="imageError" id="error-alert">
        <p>As imagens só podem ser png ou jpg</p>
        <span class="close" @click="closeAlert">x</span>
      </div>
    </div>
    <div class="row-inputs buttons-container">
      <button class="btn red" @click="cleanFields">Limpar</button>
      <button class="btn amber" @click="addItem">Cadastrar</button>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import ToggleButton from "vue-js-toggle-button";
Vue.use(ToggleButton);
export default {
  name: "GerarCardapio",
  props: {
    method: { type: Function }
  },
  data: function() {
    return {
      foodTitle: "",
      errofoodTitle: "",
      foodFlavor: "",
      errofoodFlavor: "",
      foodPrice: null,
      errofoodPrice: "",
      foodDescription: "",
      errofoodDescription: "",
      imageData: "",
      erroimageData: "",
      imageError: false,
      isDrimk: false
    };
  },
  methods: {
    processFile(event) {
      let file = event.target.files[0];
      console.log(file.type);
      if (
        file.type === "image/png" ||
        file.type === "image/jpg" ||
        file.type === "image/jpeg"
      ) {
        this.imageError = false;
        this.imageData = URL.createObjectURL(file);
      } else {
        this.imageData = "";
        this.imageError = true;
      }
    },
    closeAlert() {
      this.imageError = !this.imageError;
    },
    cleanFields() {
      this.foodTitle = "";
      this.errofoodTitle = "";
      this.foodFlavor = "";
      this.errofoodFlavor = "";
      this.foodPrice = null;
      this.errofoodPrice = "";
      this.foodDescription = "";
      this.errofoodDescription = "";
      this.imageData = "";
      this.erroimageData = "";
      this.imageError = false;
    },
    addItem() {
      let sucess = true;
      if (this.foodTitle.length <= 3 || this.foodTitle.length >= 60) {
        sucess = false;
        this.errofoodTitle = true;
      }
      if (this.foodFlavor.length <= 3 || this.foodFlavor.length >= 60) {
        sucess = false;
        this.errofoodFlavor = true;
      }
      if (this.foodPrice === null) {
        sucess = false;
        this.errofoodPrice = true;
      }
      if (sucess) {
        let data = {
          foodTitle: this.foodTitle,
          foodFlavor: this.foodFlavor,
          foodPrice: this.foodPrice,
          foodDescription: this.foodDescription,
          imageData: this.imageData,
          isDrimk: this.isDrimk
        };
        this.method(data);

        this.cleanFields();
      }
    }
  }
};
</script>

<style scoped lang="scss">
#cardapiobox {
  background-color: #fff;
  width: 60%;
  border-radius: 20px;
  box-shadow: 0px 0px 30px #740b0b45;
  padding-bottom: 1rem;
}
#header {
  display: flex;
  flex-direction: row;
  padding: 1rem 1rem 1rem 1rem;
  background-color: #ffca00;
  border-radius: 20px 20px 0px 0px;
  align-items: center;
  h3 {
    color: #a03400;
    text-align: left;
    font-style: italic;
    flex-grow: 2;
  }
  div {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-items: center;
    span {
      margin-left: 0.15rem;
      margin-right: 0.15rem;
      font-size: 12px;
      color: #a03400;
    }
  }
}
#contentInputs {
  padding: 0rem 1rem 1rem 2rem;
  display: flex;
  flex-direction: column;
  margin-top: -1.5rem;
}
.inputPd {
  font-size: 0.75rem;
  border: 1px solid #e43636;
  border-radius: 10px;
  color: #e43636;
  background-color: #fff;
  margin-left: 0.5rem;
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  padding: 10px;
}
#row-inputs {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  flex-wrap: wrap;
  .mediuminput {
    flex-grow: 2;
    flex-basis: 0;
  }
  .smallInput {
    flex-grow: 1;
    flex-basis: 0;
  }
  div {
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    span {
      color: #e43636;
      font-size: 10px;
      height: 10px;
    }
  }
}
#imagepicker {
  visibility: hidden;
  width: 0.1px;
  height: 0.1px;
}
#file-input-pd {
  font-size: 0.75rem;
  border: 1px solid #e43636;
  border-radius: 10px;
  color: #e43636;
  background-color: #fff;
  margin-left: 0.5rem;
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  padding: 10px;
}
#error-alert {
  border: 1px solid #e43636;
  border-radius: 10px;
  margin-left: 0.5rem;
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  padding: 0.5rem;
  background-color: lighten($color: #e43636, $amount: 30%);
  display: flex;
  flex-direction: row;
  color: #e43636;
  p {
    flex-grow: 2;
  }
  span {
    cursor: pointer;
    &:hover {
      color: lighten($color: #e43636, $amount: 10%);
    }
  }
}
.buttons-container {
  margin-bottom: -3rem;
}
.btn {
  height: 3rem;
  margin: 0.5rem;
  border: none;
  border-radius: 30px;
  flex: 1;
  font-weight: bold;
  text-transform: uppercase;
  min-width: 10rem;
  font-size: 1.25rem;
}
.red {
  color: #fff;
  background-color: #e43636;
}
.amber {
  color: #a03400;
  background-color: #ffca00;
}
</style>
