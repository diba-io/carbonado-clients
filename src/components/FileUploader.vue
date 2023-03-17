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
          placeholder="Get Public Key"
          v-model="publicKey"
        />
        <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="searchReferral"
          >
            GET KEY
          </button>
        </div>
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
    <div>
      <form>
        <label class="file_select"
          >SELECT FILE
          <input
            type="file"
            id="myfile"
            ref="file"
            name="myfile"
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
      console.log("READ FILE");
      this.example = this.$refs.file.files[0];
      if (
        this.example.name.includes(".png") ||
        this.example.name.includes(".jpg")
      ) {
        this.image = true;
        this.preview = URL.createObjectURL(this.example);
      } else {
        this.image = false;
      }
    },
    handleFileChange(e) {
      console.log("SELECTED A FILE onChange");
      this.$emit("input", e.target.files[0]);
      //this.value = e.target.files[0];
    },
    onFileSelection() {
      // called from the select file text
      console.log("Selected File");
    },
    async uploadTheFile() {
      console.log("form data");
      let formData = new FormData();
      formData.append("file", this.myfile.files[0]);
      await fetch("https://diba-io/carbanado-node/store/", {
        method: "POST",
        headers: {
          "Content-Type": "multipart/form-data",
        },
        body: formData,
      })
        .then(this._uploadSuccess)
        .catch(this._uploadError);
    },
    _uploadSuccess(response) {
      console.log("upload success: ", response);
    },
    _uploadError(response) {
      console.log("upload failure error,", response);
      // alert(response.data ? JSON.stringify(response.data) : response.statusText);
    },
    getSecretKey() {
      console.log("Get Secret");
    },
    uploadFile() {
      console.log("upload large file");
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
