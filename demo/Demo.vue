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
import FileChangeUpload from '../src/FileChangeUpload.vue'

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

<style>
</style>
