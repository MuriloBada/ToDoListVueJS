<template>
  <div class="Tasks">
    <div
      @click="alteraStatus(index)"
      v-for="(task, index) in tasks"
      :key="index"
      shaped
      :class="task.status ? 'Active' : 'Completed'"
    >
      <div class="text">{{ task.name }}</div>
      <span class="excluir" @click="excluir(index)">X</span>
    </div>
  </div>
</template>

<script>
import barramento from "@/barramento.js";

export default {
  data() {
    return {
      name: "",
      tasks: [],
      quantidade_total: 0,
      quantidade_ativo: 0,
      percentual: 0,
      tamanho: 0,
    };
  },
  created() {
    barramento.$on("TaskAdicionada", (nome) => {
      this.name = nome;
      this.tasks.push({
        name: this.name,
        status: true,
      });
      this.quantidade_ativo++;
      this.alteraQtdTotal();
    });
  },
  methods: {
    excluir(i) {
      if (this.tasks.length - 1 == 0) {
        this.tasks = [];
        this.percentual = 0;
        this.quantidade_ativo = 0;
        this.quantidade_total = 0;
        barramento.$emit("percentual", this.percentual);
      } else {
        this.quantidade_total--;
        if (this.tasks[i].status) {
          this.quantidade_ativo--;
          this.tasks.splice(i, 1);
        } else {
          this.tasks.splice(i, 1);
        }
        this.alteraStatus(i);
      }
    },
    alteraStatus(i) {
      if (this.tasks[i].status) {
        this.tasks[i].status = false;
        this.quantidade_ativo--;
      } else {
        this.tasks[i].status = true;
        this.quantidade_ativo++;
      }
    },
    alteraQtdTotal() {
      this.quantidade_total = this.tasks.length;
    },
  },
  watch: {
    quantidade_ativo: function () {
      if (this.quantidade_total > 0) {
        this.percentual =
          ((this.quantidade_total - this.quantidade_ativo) /
            this.quantidade_total) *
          100;
        barramento.$emit("percentual", this.percentual);
      }
    },
  },
};
</script>
<style scoped>
div {
  height: fit-content;
  width: fit-content;
}
.Tasks {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.Tasks > div {
  position: relative;
  padding: 15px;
  min-width: 250px;
  min-height: 100px;
  border-radius: 8px;
  margin: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
  align-items: center;
  user-select: none;
  cursor: crosshair;
}
.Completed {
  background-color: rgba(115, 255, 0, 0.404);
  border-left: 12px solid rgb(76, 168, 0);
}
.Active .excluir{
  background-color: rgb(167, 0, 0);
}

.Completed .excluir{
  background-color: rgb(76, 168, 0);
}

.Active {
  background-color: rgba(255, 0, 0, 0.404);
  border-left: 12px solid rgb(167, 0, 0);
}
.Completed:hover {
  background-color: rgba(84, 187, 0, 0.404);
}

.Active:hover {
  background-color: rgba(170, 0, 0, 0.404);
}

.Completed .text {
  text-decoration: line-through;
}
.excluir{
  display: flex;
  justify-content: center;
  width: 20px;
  height: 20px;
  border-radius: 10px;
  position: absolute;
  top: 10px;
  right: 10px;
}
.text {
  font-size: 2rem;
  font-weight: 200;
}

</style>