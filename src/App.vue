<template>
  <div id="app">
    <h1 class="app-title">{{title}}</h1>
    <Game />
  </div>
</template>

<script>
import Game from "./components/Game";

export default {
  name: "App",
  data: function () {
    return {
      title: "😈 Kill the MONSTER 😈",
    };
  },
  components: {
    Game,
  },
};
</script>

<style scoped>
#app {
  text-align: center;
  max-width: 960px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

.app-title {
  margin: 4px;
}

@media screen and (max-width: 600px) {
  .app-title {
    font-size: 24px;
  }
}
</style>
