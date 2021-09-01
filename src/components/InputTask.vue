<template>
  <div class="InputTask">
    <form>
      <input
        type="text"
        v-model="valor"
        placeholder="Informe o nome da tarefa"
      />
      <input type="submit" class="button" @click="alteraValor" value="+" />
    </form>
  </div>
</template>

<script>
import barramento from "@/barramento.js";
export default {
  data() {
    return {
      valor: "",
      valor2: "",
      dialog: false,
    };
  },
  methods: {
    alteraValor(event) {
      event.preventDefault();
      if (!this.valor) {
        alert("Informe o nome de uma tarefa");
      } else {
        this.valor2 = this.valor;
        this.valor = "";

        barramento.$emit("TaskAdicionada", this.valor2);
      }
    },
  },
};
</script>

<style scoped>
input {
  text-align: left;
  width: 250px;
  height: 35px;
  border: 1px solid white;
  border-radius: 5px;
  border-top-right-radius: 0px;
  border-bottom-right-radius: 0px;
  padding: 5px;
  color: white;
}

.button {
  text-align: center;
  border-radius: 5px;
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
  color: black;
  margin: 0;
  background: white;
  height: 35px;
  width: 2rem;
}
</style>