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

    // uplaodFile() {
    //   // const pubkey = this.pubKey;
    //   console.log("Public Key (nostr) :: ", this.pubkey);
    //   // run carbanodo with docker at
    //   const url = "http://0.0.0.0:7000/store/" + this.pubKey;
    //   const size = 1000000;
    //   var reader = new FileReader();
    //   var buf;
    //   var file = this.$refs.doc.files[0];
    //   reader.onload = function (e) {
    //     console.log(">> reader.onload :: ", e);
    //     buf = new Uint8Array(e.target.result);
    //     for (var i = 0; i < buf.length; i += size) {
    //       var fd = new FormData();
    //       //fd.append("pubkey", [pubkey].join("-"));
    //       fd.append("fname", [file.name, i + 1, "of", buf.length].join("-"));
    //       fd.append("data", new Blob([buf.subarray(i, i + size)]));
    //       console.log("FormData : : ", fd);
    //       var oReq = new XMLHttpRequest();
    //       oReq.open("POST", url, true);
    //       oReq.onload = function (oEvent) {
    //         // Uploaded.
    //         console.log("Uploaded : : ", oEvent);
    //       };
    //       oReq.send(fd);
    //     }
    //   };
    //   reader.readAsArrayBuffer(file);
    // },

    // async uplaodFileBAck() {
    //   console.log("localStorage pubkey: ", this.pubKey);
    //   var file = this.$refs.doc.files[0];
    //   //const chunkSize = 1000000;
    //   const chunkSize = 100000;
    //   //const chunkSize = 999600;
    //   //const url = "http://0.0.0.0:7000/store/:" + this.pubKey;
    //   // test
    //   const url = "http://127.0.0.1:3000/upload";
    //   console.log("URL : ", url);
    //   for (let start = 0; start < file.size; start += chunkSize) {
    //     const chunk = file.slice(start, start + chunkSize); // might need to remove +1
    //     const fd = new FormData();
    //     // fd.set("pubkey", this.pubKey);
    //     fd.set("data", chunk);
    //     console.log("FormData :pubkey : ", fd);

    //     await fetch(url, {
    //       method: "post",
    //       body: fd,
    //       // body: file,
    //     })
    //       .then(this._uploadSuccess)
    //       .catch(this._uplaodError);
    //   }
    // },

    async uplaodFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      var file = this.$refs.doc.files[0];
      //const chunkSize = 1000000;
      //const chunkSize = 100000;
      //const chunkSize = 999600;
      // const url = "http://0.0.0.0:7000/store/:" + this.pubKey;
      // test
      const url = "http://127.0.0.1:3000/upload/:" + this.pubKey;
      console.log("URL : ", url);
      const fd = new FormData();
      // fd.set("pubkey", this.pubKey);
      fd.set("data", file);
      console.log("FormData :pubkey : ", fd);

      const myHeaders = new Headers();
      myHeaders.append("Accept", "image/*");
      myHeaders.append("Access-Control-Allow-Origin", "http://www.example.com");

      await fetch(url, {
        method: "POST",
        mode: "no-cors",
        cache: "no-cache",
        //body: fd,
        headers: myHeaders,
        redirect: "follow",
        referrerPolicy: "no-referrer",
        body: file,
      })
        .then(this._uploadSuccess)
        .catch(this._uplaodError);
    },

    _uploadSuccess(response) {
      console.log("response UPLOAD SUCCESS data: ", response);
      this.linkAddress = response.url;
      this.status = response.status + " : " + response.statusText;
    },

    _uplaodError(response) {
      console.log("COULDN'T UPLOAD FILE data _error,", response);
      this.status = response.status + " : " + response.statusText;
      // alert(response.data ? JSON.stringify(response.data) : response.statusText);
    },

    async deleteFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      var file = this.$refs.doc.files[0];
      // to delete a file we need the blake3hash for file_name
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
