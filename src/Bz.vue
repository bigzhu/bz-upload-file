<style lang=less>
  .hide{
    display: none
  }
</style>

<template>
  <input class="image input" type="file" @change="changeFile" :accept="accept"/>
</template>

<script>
  import toastr from 'toastr'
  import $ from 'jquery'
  export default {
    props: {
      change_call_back: {
        type: Function
      },
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
    ready () {
      this.file_input = $(this.$el)
    },
    methods: {
      changeFile: function (e) {
        if (this.change_call_back) {
          this.change_call_back(e)
        }
        this.uploadFile()
      },
      uploadFile: function () {
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
              success: (
                function (_this) {
                  return function (data, status, response) {
                    if (!data.success) {
                      throw new Error(data.msg)
                    } else {
                      // toastr.info('保存成功')
                      // _this.pre_img.attr('src', data.file_path)
                      if (_this.done_call_back) {
                        _this.done_call_back(data.file_path)
                      }
                    }
                  }
                }
              )(this),
              error: function (error_info) {
                toastr.error(error_info)
                // throw new Error(error_info)
              }
            }
          )
        }
      }
    },
    computed: {
    }
  }
</script>
