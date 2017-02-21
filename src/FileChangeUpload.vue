<template>
<div class="file-change-upload">
  <label :for="id || name">
    <input type="file"
      :name="name"
      :id="id || name"
      :accept="accept"
      :multiple="multiple"
      @change="onChange">
    <slot></slot>
  </label>
</div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    },
    id: String,
    action: {
      type: String,
      required: true
    },
    accept: String,
    multiple: String,
    headers: Object,
    data: Object,
    method: String,
  },
  data() {
    return {
      files: []
    }
  },
  methods: {
    onChange(event) {
      this.files = document.getElementById(this.id || this.name).files
      this.files = Array.prototype.slice.call(this.files, 0)
      if (this.files.length) {
        let arrayOfPromises = this.files.map(file => this._handleUpload(file))
        Promise.all(arrayOfPromises).then(allFiles => {
          this.$emit('onAllFilesUploaded', allFiles)
        }).catch(err => {
          this.$emit('onFileError', this.files, err)
        })
      } else {
        let err = new Error('No files to upload for this field')
        this.$emit('onFileError', this.files, err)
      }
    },
    _handleUpload(file) {
      this.$emit('beforeFileUpload', file)
      let form = new FormData()
      let xhr = new XMLHttpRequest()
      try {
        form.append(this.name, file)
        for(let name in this.data) {
          form.append(name, this.data[name])
        }
      } catch (err) {
        this.$emit('onFileError', file, err)
        return
      }
      return new Promise((resolve, reject) => {
        xhr.upload.addEventListener('progress', e => {
          e.percent = (e.loaded / e.total) * 100
          this.$emit('onFileProgress', e)
        }, false)

        let onerror = () => {
          let err = new Error(xhr.statusText)
          this.$emit('onFileError', file, err)
          reject(err)
        }

        xhr.onreadystatechange = () => {
          if (xhr.readyState < 4) {
            return
          }
          if (xhr.status < 400) {
            let res = JSON.parse(xhr.responseText)
            this.$emit('onFileUpload', file, res)
            resolve(file)
          } else {
            onerror()
          }
        }

        xhr.onerror = onerror

        xhr.open(this.method || 'POST', this.action, true)
        if (this.headers) {
          for(var header in this.headers) {
            xhr.setRequestHeader(header, this.headers[header])
          }
        }
        xhr.send(form)
        this.$emit('afterFileUpload', file)
      })
    }
  }
}
</script>

<style scoped>

</style>
