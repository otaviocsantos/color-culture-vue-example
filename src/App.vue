<template>
  <div id="app">
    <div class="box-content">
      <CultureExample/>
      <ColorFunctions/>
    </div>
  </div>
</template>

<style>
</style>


<script>
import ColorFunctions from "./components/ColorFunctions.vue";
import CultureExample from "./components/CultureExample.vue";

export default {
  name: "app",
  components: {
    ColorFunctions,
    CultureExample
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Open+Sans');
body{
  font-family: 'Open Sans', sans-serif;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
}
.box-content{
  max-width: 960px;
}
ul {
  list-style: none;
  padding: 0;
}
h1, h2, h3, h4, h5, h6{
  font-family: 'Open Sans', sans-serif;
}

</style>
