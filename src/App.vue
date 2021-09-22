<template>
  <div id="app">
    <Game />
    <div class='instructions'>Use <b>arrow keys</b> to play the game</div>
  </div>
</template>

<script>
import Game from "./components/Game.vue";

export default {
  name: "App",
  components: {
    Game
  }
};
</script>

<style>
body, html {
  overflow: hidden;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  height: 100vh;
  width: 100vw;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  height: 100%;
  justify-content: center;
  align-items: center;
  background: url(https://images.pexels.com/photos/1242348/pexels-photo-1242348.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260)
    no-repeat;
  background-size: cover;
}
.instructions {
  position: absolute;
  bottom: 20px;
}
</style>