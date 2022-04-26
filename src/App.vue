<template>
  <div id="app">
    <div class="upload-example">
      <Cropper
        ref="cropper"
        class="upload-example-cropper"
        :src="image"
        :stencil-component="$options.components.Stencil"
      />
      <div class="button-wrapper">
        <span class="button" @click="$refs.file.click()">
          <input
            type="file"
            ref="file"
            @change="uploadImage($event)"
            accept="image/*"
          />
          Upload image
        </span>
        <span class="button" @click="cropImage">Crop image</span>
      </div>
    </div>
  </div>
</template>

<script>
import { Cropper } from "vue-advanced-cropper";
import "vue-advanced-cropper/dist/style.css";

import Stencil from "./Stencil";

export default {
  data() {
    return {
      image:
        "https://images.pexels.com/photos/1451124/pexels-photo-1451124.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940",
    };
  },
  components: {
    Cropper,
    Stencil,
  },
  methods: {
    cropImage() {
      const result = this.$refs.cropper.getResult();
      const newTab = window.open();
      newTab.document.body.innerHTML = `<img src="${result.canvas.toDataURL(
        "image/jpeg"
      )}"></img>`;
    },
    uploadImage(event) {
      // Reference to the DOM input element
      var input = event.target;
      // Ensure that you have a file before attempting to read it
      if (input.files && input.files[0]) {
        // create a new FileReader to read this image and convert to base64 format
        var reader = new FileReader();
        // Define a callback function to run, when FileReader finishes its job
        reader.onload = (e) => {
          // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
          // Read image as base64 and set to imageData
          this.image = e.target.result;
        };
        // Start the reader job - read file as a data url (base64 format)
        reader.readAsDataURL(input.files[0]);
      }
    },
  },
};
</script>

<style>
.upload-example-cropper {
  border: solid 1px #EEE;
  min-height: 300px;
  width: 100%;
}

.button-wrapper {
  display: flex;
  justify-content: center;
  margin-top: 17px;
}

.button {
  color: white;
  font-size: 16px;
  padding: 10px 20px;
  background: #3fb37f;
  cursor: pointer;
  transition: background 0.5s;
  font-family: Open Sans, Arial;
  margin: 0 10px;
}

.button:hover {
  background: #38d890;
}

.button input {
  display: none;
}
</style>
