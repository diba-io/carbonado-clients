<template>
  <img alt="Diba-Logo" id="img-size" src="./assets/diba-logo.jpg" />
  <FileUploader msg="Carbonado HTTP API" />
</template>

<script>
import FileUploader from "./components/FileUploader.vue";

export default {
  name: "App",
  components: {
    FileUploader,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: rgb(16, 15, 15);
  margin: 0px;
  padding: 0px;
}
#img-size {
  width: 100px;
  margin-top: 15px;
}
</style>
