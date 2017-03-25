<template>
  <div @click="click" class="ui button">
    <slot></slot>
    <input type="file" @change="changeFile" :accept="accept" style="display: none"/>
  </div>
</template>

<script>
  import $ from 'jquery'
  export default {
    props: {
      done_call_back: {
        type: Function
      },
      upload_url: {
        type: String,
        default: '/api_file_upload'
      },
      accept: {
        type: String,
        default: '*'
      }
    },
    components: {
    },
    data: function () {
      return {
        file_input: null
      }
    },
    mounted: function () {
      this.$nextTick(function () {
        this.file_input = $(this.$el).find('input')
      })
    },
    methods: {
      changeFile: function (e) {
        this.$emit('change_file', e)
        this.uploadFile()
      },
      uploadFile: function () {
        let file
        let fd
        fd = new window.FormData()
        let self = this
        file = this.file_input[0].files[0]
        console.log(file)
        if (file) {
          fd.append('file', file)
          return window.fetch(this.upload_url, {
            method: 'post',
            body: fd
          })
          .then(function (response) {
            if (response.status !== 200) {
              throw new Error(response.url + ' ' + response.status + ' ' + response.statusText)
            }
            return response.json()
          })
          .then(function (data) {
            if (data.error !== '0') {
              console.log(self.upload_url + ' error: ' + data.error)
              throw new Error(data.error)
            }
            self.$emit('upload_done', data.file_path)
            return data
          })
          .catch(function (error) {
            console.log('Request failed', error)
          })
        }
      },
      click: function () {
        $(this.$el).find('input').click()
      }
    },
    computed: {
    }
  }
</script>
