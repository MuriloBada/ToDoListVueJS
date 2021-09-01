<template>
  <div class="ProgressBar">
    <div class="life-bar">
      <div class="life" :style="{ width: progresso + '%' }"></div>
    </div>
    <div class="progresso">{{ progresso.toFixed(0) }}%</div>
  </div>
</template>

<script>
import barramento from "@/barramento.js";

export default {
  data() {
    return {
      progresso: 0,
    };
  },
  created() {
    barramento.$on("percentual", (percent) => {
        this.progresso = percent;
    });
  },
};
</script>

<style scoped>
.ProgressBar {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 2%;
  margin-bottom: 2%;
}
.life-bar {
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 80%;
  height: 25px;
  border: 1px solid #aaa;
}
.life {
  height: 100%;
  background-color: white;
}
.progresso {
  margin-top: 10px;
  font-weight: bold;
  font-size: 18pt;
  float: 1;
}

.life, .life-bar{
  border-radius: 8px;
}
</style>