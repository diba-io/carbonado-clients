<template>
  <div class="bg-form">
    <div class="hello">
      <h1>{{ msg }}</h1>
    </div>
    <h4 id="card-header">File Uploader</h4>
    <div>
      <div class="search-row">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Public Key in Hex Format ( Nostr )"
          v-model="pubKey"
        />
        <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="storeKey"
          >
            STORE KEY
          </button>
        </div>
      </div>
      <div class="search-row">
        <input
          type="text"
          class="form-control"
          placeholder="Returned Link Address"
          v-model="linkAddress"
        />
        <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="copyLlink"
          >
            Copy
          </button>
        </div>
      </div>
    </div>
    <div class="file-row">
      <label class="file_select">
        <input
          type="file"
          id="file-uploader"
          ref="doc"
          name="myfile"
          @change="uplaodFile()"
        />
      </label>
      <button @click="uploadTheFile">Upload</button>
    </div>
    <div>
      <div class="search-row">
        <input
          type="text"
          class="form-control"
          placeholder="Delete File Link Address"
          v-model="linkAddress"
        />
        <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="deleteFile"
          >
            Delete
          </button>
        </div>
      </div>
    </div>
    <div class="search-row">
      <input
        type="text"
        class="form-control"
        placeholder="Status"
        v-model="status"
      />
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
      id: -1,
      pubKey: "",
      files: [],
      input: null,
      image: null,
      fileupload: [],
      linkAddress: "",
      response: "",
      status: "Upload Status",
    };
  },

  methods: {
    storeKey() {
      localStorage.setItem("pubkey", JSON.stringify(this.pubKey));
      var pk = JSON.parse(localStorage.getItem("pubkey"));
      this.pubKey = pk;
      console.log("Store Public Key", this.pubKey);
    },

    async uplaodFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      var file = this.$refs.doc.files[0];
      const chunkSize = 1000000;
      for (let start = 0; start < file.size; start += chunkSize) {
        const url = "http://127.0.0.1:7000/store/" + 0 + 3 + this.pubKey;
        console.log("URL : ", url);
        const chunk = file.slice(start, start + chunkSize);
        const fd = new FormData();
        fd.set("data", chunk);
        console.log("FormData :pubkey : ", fd);

        const myHeaders = new Headers();
        myHeaders.append("Accept", "image/*");
        myHeaders.append("Access-Control-Allow-Origin", "*");

        await fetch(url, {
          method: "POST",
          mode: "no-cors",
          cache: "no-cache",
          headers: myHeaders,
          redirect: "follow",
          referrerPolicy: "no-referrer",
          body: fd,
        })
          .then(this._uploadSuccess)
          .catch(this._uplaodError);
      }
    },

    _uploadSuccess(response) {
      console.log("response UPLOAD SUCCESS data: ", response);
      this.linkAddress = response.url;
      this.status = response.status + " : " + response.statusText;
    },

    _uplaodError(response) {
      console.log("COULDN'T UPLOAD FILE data _error,", response);
      this.status = response.status + " : " + response.statusText;
    },

    async deleteFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      var file = this.$refs.doc.files[0];
      const url = "http://0.0.0.0:7000/remove_file/:" + this.pubKey + file;
      console.log("URL : ", url);

      await fetch(url, {
        method: "delete",
      })
        .then(this._deleteFileSuccess)
        .catch(this._deleteFileError);
    },
  },

  computed: {
    valid() {
      return this.files.length <= this.max;
    },
  },

  mounted() {
    this.pubKey = JSON.parse(localStorage.getItem("pubkey"));
    console.log("mounted pubkey set to: ", this.pubKey);
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
  margin-top: 7px;
  margin-bottom: 7px;
  margin-left: 7px;
  width: 70%;
}

button {
  width: 110px;
  background-color: #42b983;
}

.file-row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-top: 10px;
}
</style>
