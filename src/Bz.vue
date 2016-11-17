<template>
  <div @click="click" class="ui button"><slot></slot>
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
        let self = this
        var fd, file
        fd = new window.FormData()
        file = this.file_input[0].files[0]
        if (file) {
          fd.append('file', file)
          return $.ajax(
            {
              url: this.upload_url,
              type: 'POST',
              data: fd,
              processData: false,
              contentType: false,
              success: function (data, status, response) {
                if (!data.success) {
                  throw new Error(data.msg)
                } else {
                  self.$emit('upload_done', data.file_path)
                }
              },
              error: function (error_info) {
                throw new Error(error_info)
              }
            }
          )
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
