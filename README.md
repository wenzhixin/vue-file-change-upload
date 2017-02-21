# vue-file-change-upload

vue-file-change-upload is an ui component library base on Vue.js(2.x).

# Usage

```html
<template>
  <file-change-upload name="file" action="upload.php"
    @onAllFilesUploaded="onAllFilesUploaded"
    @beforeFileUpload="beforeFileUpload"
    @onFileProgress="onFileProgress"
    @afterFileUpload="afterFileUpload"
    @onFileError="onFileError">
  </file-change-upload>
</template>
<script>
import FileUpload from '../src/FileChangeUpload.vue'

export default {
  components: {
    FileChangeUpload
  },
  methods: {
    onAllFilesUploaded(files) {
      console.log('onAllFilesUploaded', files)
    },
    beforeFileUpload(file) {
      console.log('beforeFileUpload', file)
    },
    onFileProgress(event) {
      console.log('onFileProgress', event)
    },
    afterFileUpload(file) {
      console.log('afterFileUpload', file)
    },
    onFileError(file, err) {
      console.log('onFileError', file, err)
    }
  }
}
</script>
```

## Installation

```
npm install vue-file-change-upload
```

## Example

* [Demo](https://github.com/wenzhixin/vue-file-change-upload/tree/master/demo)
* [Live Example](https://wenzhixin.github.io/vue-file-change-upload/demo)

## Documentation

### props

* name: String, The file input name, required
* id: String, The file input id
* action: String, The upload server address, required
* accept: String
* multiple: String
* headers: Object, The headers send to server
* data: Object, The data send to server
* method: String, Default is 'POST'

### events

* onAllFilesUploaded: Trigger when all files uploaded
* onFileUpload: Trigger when one file uploaded
* beforeFileUpload: Trigger before one file upload
* onFileProgress: Trigger when file uploading
* afterFileUpload: Trigger after one file upload
* onFileError: Trigger when something error
