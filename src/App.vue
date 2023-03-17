<template>
  <img alt="Vue logo" id="img-size" src="./assets/diba-logo.jpg" />
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
  margin-top: 60px;
}
#img-size {
  width: 100px;
  margin-top: 10px;
}
</style>
