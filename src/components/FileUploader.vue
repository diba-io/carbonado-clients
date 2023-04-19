<template>
  <div class="bg-form">
    <!-- <h2 id="card-header">{{ msg }}</h2> -->
    <div>
      <div class="file-row">
        <div class="form-group" id="left-group-width">
          <input
            type="text"
            class="form-control"
            id="pad"
            placeholder="Enter Public Key in Hex Format ( Nostr )"
            v-model="pubKey"
          />
        </div>
        <div class="form-group" id="group-width">
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
      </div>
      <div class="file-row">
        <div class="form-group" id="choose-group-width">
          <label id="enhance-text" for="file-uploader"
            >Choose af File to Upload</label
          >
          <label class="file_select">
            <input
              type="file"
              id="file-uploader"
              ref="doc"
              name="myfile"
              @change="uplaodFile()"
            />
          </label>
        </div>
        <div class="form-group" id="right-group-width">
          <label id="enhance-text" for="ranges">Slices</label>
          <input
            type="text"
            class="form-control"
            id="ranges"
            placeholder="Enter Public Key in Hex Format ( Nostr )"
            v-model="slice_count"
          />

          <!-- <button @click="uploadTheFile">Upload</button> -->
        </div>
      </div>
      <div class="file-row">
        <div class="form-group" id="left-group-width">
          <input
            type="text"
            class="form-control"
            placeholder="Returned Link Address"
            v-model="linkAddress"
          />
        </div>
        <div class="form-group" id="group-width">
          <div class="search-btn">
            <button
              class="btn btn-outline-secondary"
              type="button"
              @click="copyLlink"
            >
              Link
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- <div class="file-row">
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
    </div> -->
    <div class="file-row">
      <div class="form-group" id="left-group-width">
        <input
          type="text"
          class="form-control"
          placeholder="Get File"
          v-model="blake3_hash"
        />
      </div>
      <div class="form-group" id="group-width">
        <div class="search-btn">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="getFile"
          >
            Get File
          </button>
        </div>
      </div>
    </div>

    <div class="block-row">
      <div class="form-group" id="full-group-width">
        <input
          type="text"
          class="form-control"
          placeholder="Enter Public Key in Hex Format ( Nostr )"
          v-model="pubKey"
        />
      </div>

      <div class="slice-row">
        <div class="form-group">
          <label id="enhance-text" for="file-uploader">Start</label>
          <input
            type="text"
            class="form-control"
            id="range-width"
            placeholder="Start"
            v-model="index_start"
          />
        </div>
        <div class="form-group">
          <label id="enhance-text" for="file-uploader">Range</label>
          <input
            type="text"
            class="form-control"
            id="range-width"
            placeholder="Range"
            v-model="range_end"
          />
        </div>
        <div class="form-group" id="group-width">
          <label id="enhance-text" for="file-uploader">.</label>
          <div class="search-btn">
            <button
              class="btn btn-outline-secondary"
              type="button"
              @click="getFileSlice"
            >
              GET SLICE
            </button>
          </div>
        </div>
      </div>
    </div>
    <div>
      <div class="file-row">
        <div class="form-group" id="left-group-width">
          <input
            type="text"
            class="form-control"
            placeholder="Delete File Link Address"
            v-model="linkAddress"
          />
        </div>
        <div class="form-group" id="group-width">
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
    </div>
    <div class="file-row">
      <div class="form-group" id="full-group-width">
        <input
          type="text"
          class="form-control"
          placeholder="Status"
          v-model="status"
        />
      </div>
    </div>
    <div class="file-row">
      <div class="form-group" id="full-group-width">
        <input
          type="text"
          class="form-control"
          placeholder="Return Snippet/File"
          v-model="response"
        />
      </div>
    </div>
    <div class="file-row">
      <div class="form-group" id="full-group-width">
        <div id="target"></div>
      </div>
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
      snippet: "File Read Content",
      index_start: 65536,
      range_end: 8192,
      slice_count: 0,
      blake3_hash: "",
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
        myHeaders.append("Accept", "*");
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
          .then((response) => response)
          .then((result) => {
            // Do things with result
            console.log(" UPLOAD -> RESPONSE:: ", result);
            this._uploadSuccess(result);
          });
        //.catch(this._uplaodError);
      }
      //}
    },

    async _uploadSuccess(response) {
      console.log("response UPLOAD SUCCESS data: ", response);
      //   this.linkAddress = response.url;
      //   this.status = response.status + " : " + response.statusText;
      //   this.response = response.data;

      const url = "http://127.0.0.1:7000/last_hash/" + 0 + 3 + this.pubKey;
      console.log("Last Hash URL : ", url);

      // ON UPLOAD SUCCESS GET LAST HASH
      await fetch(url, {
        method: "get",
      })
        .then((response) => response.body)
        .then((rb) => {
          const reader = rb.getReader();

          return new ReadableStream({
            start(controller) {
              // The following function handles each data chunk
              function push() {
                // "done" is a Boolean and value a "Uint8Array"
                reader.read().then(({ done, value }) => {
                  // If there is no more data to read
                  if (done) {
                    console.log("done", done);
                    controller.close();
                    return;
                  }
                  // Get the data and send it to the browser via the controller
                  controller.enqueue(value);
                  // Check chunks by logging to the console
                  console.log("LAST HASH DONE :: ", done);
                  console.log("LAST HASH VALUE :: ", value);
                  // this.response = value;
                  push();
                });
              }

              push();
            },
          });
        })
        .then((stream) =>
          // Respond with our stream
          new Response(stream, {
            headers: { "Content-Type": "text/html" },
          }).text()
        )
        .then((result) => {
          // Do things with result
          console.log("GET LAST HASH -> RESULT:: ", result);
          this.linkAddress = result;
          this.blake3_hash = result;
          //this._getFileSuccess(result);
        });
    },

    _uplaodError(response) {
      console.log("COULDN'T UPLOAD FILE data _error,", response);
      this.status = response.status + " : " + response.statusText;
    },

    async deleteFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      //let file = this.$refs.doc.files[0];
      const url = "http://127.0.0.1:7000/remove_file/:" + 0 + 3 + this.pubKey;
      console.log("URL : ", url);

      await fetch(url, {
        method: "delete",
      })
        .then(this._deleteFileSuccess)
        .catch(this._deleteFileError);
    },

    _deleteFileSuccess(response) {
      console.log("DELETE SUCCESS data: ", response);
      this.linkAddress = response.url;
      this.status = response.status + " : " + response.statusText;
    },

    _deleteFileError(response) {
      console.log("COULDN'T DELETE FILE: ", response);
      this.status = response.status + " : " + response.statusText;
    },

    async getFile() {
      console.log("localStorage pubkey: ", this.pubKey);
      //let key = 0 + 3 + this.pubKey;
      let blake3_hash = this.blake3_hash;
      // const url = "http://127.0.0.1:7000/retrieve/:" + key; //+
      const url =
        "http://127.0.0.1:7000/retrieve/" +
        0 +
        3 +
        this.pubKey +
        "/" +
        blake3_hash;
      // "?pk=" +
      // key +
      // "&blake3_hash=" +
      // blake3_hash;
      console.log(" >>>>>>>> URL : ", url);

      await fetch(url, {
        method: "get",
      })
        .then((response) => response.body)
        .then((rb) => {
          const reader = rb.getReader();

          return new ReadableStream({
            start(controller) {
              // The following function handles each data chunk
              function push() {
                // "done" is a Boolean and value a "Uint8Array"
                reader.read().then(({ done, value }) => {
                  // If there is no more data to read
                  if (done) {
                    console.log("done", done);
                    controller.close();
                    return;
                  }
                  // Get the data and send it to the browser via the controller
                  controller.enqueue(value);
                  // Check chunks by logging to the console
                  console.log("DONE :: ", done);
                  console.log("VALUE :: ", value);
                  // this.response = value;
                  push();
                });
              }

              push();
            },
          });
        })
        .then((stream) =>
          // Respond with our stream
          new Response(stream, {
            headers: { "Content-Type": "text/html" },
          }).text()
        )
        .then((result) => {
          // Do things with result
          console.log(" MY STREAM FINAL RESULT:: ", result);
          this.response = result;
          this._getFileSuccess(result);
        })
        .catch(this._getFileError);
    },

    _getFileSuccess(result) {
      this.response = result;
    },

    _getFileError(response) {
      console.log("FILE ERROR: ", response);
      this.status = response.status + " : " + response.statusText;
      this.response = response.status + " : " + response;
    },

    async getFileSlice() {
      console.log("localStorage pubkey: ", this.pubKey);
      let key = 0 + 3 + this.pubKey;
      let blake3_hash = this.blake3_hash;
      let index_start = this.index_start;
      let range_end = this.range_end;
      //let chain = key + ":3:7";
      const url =
        "http://127.0.0.1:7000/slice/" +
        "?pk=" +
        key +
        "&blake3_hash=" +
        blake3_hash +
        "&index_start=" +
        index_start +
        "&range_end=" +
        range_end;
      console.log(" >>>>>>>> URL : ", url);

      await fetch(url, {
        method: "get",
      })
        .then(this._getFileSliceSuccess)
        .catch(this._getFileSliceError);
    },

    _getFileSliceSuccess(response) {
      console.log("FILE SLICE SUCCESS data: ", response);
      this.linkAddress = response.url;
      this.status = response.status + " : " + response.statusText;
      this.response = response;
    },

    _getFileSliceError(response) {
      console.log("FILE SLICE ERROR: ", response);
      this.status = response.status + " : " + response.statusText;
      this.response = response.status + " : " + response;
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

#ranges {
  width: 15%;
}
@media only screen and (min-width: 1025px) {
  .bg-form {
    display: flex;
    flex-direction: column;
    /* justify-content: center; */
    /* align-items: center; */
    height: 150vh;
    background-color: rgb(128, 128, 128, 0.8);
    margin-left: 10%;
    margin-right: 10%;
    padding-top: 50px;
    padding-left: 15px;
    padding-right: 15px;
  }

  .file-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 10px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
  }
  .slice-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .block-row {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-top: 0px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
    margin-top: 10px;
  }

  #left-group-width {
    width: 60%;
  }
  #range-width {
    width: 75%;
    padding-left: 10px;
    padding-right: 10px;
  }

  #right-group-width {
    width: 20%;
  }

  #full-group-width {
    width: 100%;
  }

  label {
    color: rgb(93, 90, 90);
    opacity: 1;
    font-weight: 400;
    font-size: medium;
    display: block;
    padding-top: 5px;
    margin-right: -10px;
  }

  #pad {
    padding-left: 5px;
    padding-right: 5px;
  }
}
@media only screen and (min-width: 768px) and (max-width: 1024px) {
  .bg-form {
    display: flex;
    flex-direction: column;
    /* justify-content: center; */
    /* align-items: center; */
    height: 150vh;
    background-color: rgb(128, 128, 128, 0.8);
    margin-left: 1%;
    margin-right: 1%;
    padding-top: 5%;
    padding-left: 15px;
    padding-right: 15px;
  }
  .file-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 0px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding: 0vmin;
  }

  #left-group-width {
    width: 60%;
  }
  #center-group-width {
    width: 15%;
  }

  #right-group-width {
    width: 20%;
  }

  #full-group-width {
    width: 100%;
  }
  .bg-form {
    display: flex;
    flex-direction: column;
    /* justify-content: center; */
    /* align-items: center; */
    height: 150vh;
    background-color: rgb(128, 128, 128, 0.8);
    margin-left: 1%;
    margin-right: 1%;
    padding-top: 50px;
    padding-left: 15px;
    padding-right: 15px;
  }

  .file-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 10px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
  }
  .slice-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .block-row {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-top: 0px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
    margin-top: 10px;
  }

  #left-group-width {
    width: 60%;
  }
  #range-width {
    width: 75%;
    padding-left: 10px;
    padding-right: 10px;
  }

  #right-group-width {
    width: 20%;
  }

  #full-group-width {
    width: 100%;
  }

  label {
    color: rgb(93, 90, 90);
    opacity: 1;
    font-weight: 400;
    font-size: medium;
    display: block;
    padding-top: 5px;
    margin-right: -10px;
  }

  #pad {
    padding-left: 5px;
    padding-right: 5px;
  }
}

@media only screen and (min-width: 320px) and (max-width: 767px) {
  .bg-form {
    display: flex;
    flex-direction: column;
    /* justify-content: center; */
    /* align-items: center; */
    height: 150vh;
    background-color: rgb(128, 128, 128, 0.8);
    margin-left: 1%;
    margin-right: 1%;
    padding-top: 50px;
    padding-left: 15px;
    padding-right: 15px;
  }

  .file-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 10px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
  }
  .slice-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .block-row {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-top: 0px;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0.5em;
    box-shadow: 0 0 0.25em rgba(0, 0, 0, 0.25);
    box-sizing: border-box;
    padding-left: 8px;
    padding-right: 8px;
    margin-top: 10px;
  }

  #left-group-width {
    width: 60%;
  }
  #range-width {
    width: 75%;
    padding-left: 10px;
    padding-right: 10px;
  }

  #right-group-width {
    width: 20%;
  }

  #full-group-width {
    width: 100%;
  }

  label {
    color: rgb(93, 90, 90);
    opacity: 1;
    font-weight: 400;
    font-size: medium;
    display: block;
    padding-top: 5px;
    margin-right: -10px;
  }

  #pad {
    padding-left: 5px;
    padding-right: 5px;
  }
}
</style>
