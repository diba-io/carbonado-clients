<template>
  <div class="diba-header">
    <img
      alt="mini-carbonado-diamond"
      id="diamond-size"
      src="./assets/mini-diamond.png"
    />
    <div>Carbonado</div>
    <img
      alt="Diba-Logo"
      id="diba-logo-size"
      src="./assets/diba-logo-white.png"
    />
  </div>

  <FileUploader msg="" />
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
body {
  background-color: black;
}
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

#header-login-button {
  color: white;
  text-align: center;
  background-color: limegreen;
  border-top-right-radius: 20pt;
  border-top-left-radius: 20pt;
  border-left: 2px solid rgb(1, 54, 25, 0.5);
  border-bottom-right-radius: 20pt;
  border-bottom-left-radius: 20pt;
  border-left: 0pt solid rgb(1, 54, 25, 0.5);
  border-right: 2pt solid rgb(1, 54, 25, 0.5);
  border-bottom: 3pt solid rgb(1, 54, 25, 0.5);
  background: linear-gradient(-45deg, #1c49ee 0%, #22f74c 100%);
  height: 30pt;
  padding-left: 15px;
  padding-right: 15px;
  margin-left: 0px;
}
#img-size {
  width: 100px;
  margin-top: 5px;
}
#diamond-size {
  width: 50px;
  margin-top: 0px;
  margin-left: 10px;
}

#diba-logo-size {
  width: 100px;
  margin-top: 5px;
  margin-right: 15px;
}
@media only screen and (min-width: 1025px) {
  .diba-header {
    position: fixed;
    z-index: 10;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    /* justify-content: center; */
    align-items: center;
    width: 99vw;
    margin-top: -8px;
    margin-left: -8px;
    margin-bottom: 0px;
    margin-right: 10px;
    color: silver;
    font-size: 1.8em;
    font-weight: 400;
    font-family: Verdana, sans-serif;
    background-color: rgb(30, 31, 30);
    border-right: 0pt solid rgb(1, 54, 25, 0.4);
    border-bottom: 1pt solid rgb(1, 54, 25, 0.4);
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }
}
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  .diba-header {
    position: fixed;
    z-index: 10;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    /* justify-content: center; */
    align-items: center;
    width: 99vw;
    margin-top: -8px;
    margin-left: -8px;
    margin-bottom: 0px;
    margin-right: 10px;
    color: silver;
    font-size: 1.8em;
    font-weight: 400;
    font-family: Verdana, sans-serif;
    background-color: rgb(30, 31, 30);
    border-right: 0pt solid rgb(1, 54, 25, 0.4);
    border-bottom: 1pt solid rgb(1, 54, 25, 0.4);
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }
}
@media only screen and (min-width: 360px) and (max-width: 767px) {
  .diba-header {
    position: fixed;
    z-index: 10;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    /* justify-content: center; */
    align-items: center;
    width: 99vw;
    margin-top: -8px;
    margin-left: -8px;
    margin-bottom: 0px;
    margin-right: 10px;
    color: silver;
    font-size: 1.8em;
    font-weight: 400;
    font-family: Verdana, sans-serif;
    background-color: rgb(30, 31, 30);
    border-right: 0pt solid rgb(1, 54, 25, 0.4);
    border-bottom: 1pt solid rgb(1, 54, 25, 0.4);
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }
}
</style>
