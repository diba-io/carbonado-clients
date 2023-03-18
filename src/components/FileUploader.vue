<template>
  <div class="bg-form">
    <div class="hello">
      <h1>{{ msg }}</h1>
    </div>
    <h4 id="card-header">File Uploader</h4>
    <!-- <div class="card-body"> -->
    <!-- <div class="list row"> -->
    <div>
      <div class="search-row">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Public Key ( Nostr )"
          v-model="publicKey"
        />
        <!-- <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="searchReferral"
          >
            GET KEY
          </button>
        </div> -->
      </div>
      <div class="search-row">
        <input
          type="text"
          class="form-control"
          placeholder="Display Address"
          v-model="address"
        />
        <!-- <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="searchReferral"
          >
            Search
          </button>
        </div> -->
      </div>
    </div>
    <div class="file-row">
      <form>
        <label class="file_select">
          <!-- <input
            type="file"
            id="file-uploader"
            ref="doc"
            name="myfile"
            @change="readFile()"
          /> -->
          <input
            type="file"
            id="file-uploader"
            ref="doc"
            @change="readFile()"
          />
        </label>
        <!-- <input type="file" ref="file" @change="readFile()" /> -->
        <button class="upload-btn" id="upload-button" @click="uploadTheFile">
          Upload
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "FileUploader",
  props: {
    msg: String,
    max: {
      type: Number,
      default: 1,
    },
    value: Array,
  },
  data() {
    return {
      publicKey: "",
      files: [],
      input: null,
      image: null,
      fileupload: [],
      address: "",
    };
  },

  methods: {
    readFile() {
      // run carbanodo with docker at
      const url = "https://localhost:8000/carbanado-node/store/";
      // const url = "https://diba-io/carbanado-node/store/";
      const size = 100000; // started with 40000
      var reader = new FileReader();
      var buf;
      var file = this.$refs.doc.files[0];
      reader.onload = function (e) {
        console.log(">> reader.onload :: ", e);
        buf = new Uint8Array(e.target.result);
        for (var i = 0; i < buf.length; i += size) {
          var fd = new FormData();
          fd.append("fname", [file.name, i + 1, "of", buf.length].join("-"));
          fd.append("data", new Blob([buf.subarray(i, i + size)]));

          console.log("FormData : : ", fd);
          // debug here
          var oReq = new XMLHttpRequest();
          oReq.open("POST", url, true);
          oReq.onload = function (oEvent) {
            // Uploaded.
            console.log("Uploaded : : ", oEvent);
          };
          oReq.send(fd);
        }
      };

      reader.readAsArrayBuffer(file);
    },
  },
  computed: {
    valid() {
      return this.files.length <= this.max;
    },
  },
  watch: {
    files(files) {
      this.$emit("input", files);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.bg-form {
  height: 100vh;
  background-color: rgb(128, 128, 128, 0.8);
  margin: 10px;
  padding: 10px;
}
.search-row {
  display: flex;
  flex-direction: row;
  padding-left: 10px;
}

.file_select {
  color: whitesmoke;
  background-color: rgb(0, 0, 0, 0.6);
  font-size: 0.9em;
  font-weight: 400;
  font-family: Verdana, sans-serif;
  border-radius: 0.6rem;
  border: 1px solid rgb(128, 128, 128, 0.8);
  padding: 13px;
  margin-top: 20px;
  margin-left: 6px;
}
.upload-btn {
  margin-top: -2px;
}

.file-select-row {
  display: flex;
}

/* .file-select > .select-button {
  width: 100px;
  padding: 10px;

  color: white;
  background-color: #2ea169;

  border-radius: 0.3rem;

  text-align: center;
  font-weight: bold;
} */
</style>
