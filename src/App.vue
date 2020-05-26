<template>
  <div id="app">
    <TodoComponent />
  </div>
</template>

<script>
import TodoComponent from "./components/TodoComponent";

export default {
  name: "App",
  components: {
    TodoComponent
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  justify-content: center;
  margin-top: 50px;
}

/* Media query for screen less than 700px */
@media only screen and (max-device-width: 700px) and (-webkit-min-device-pixel-ratio: 2) {
#app {
  margin-top: 25px;
}
}
</style>
