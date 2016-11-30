<template>
  <div>
    <doc :name="name"
      :desc="desc"
      :parm_desc="parm_desc"
      :parms="parms"
      :code="code"
      >
      <bz @change_file="change_call_back" accept="pdf/*" @upload_done="call_back">上传附件</bz>
    </doc>
  </div>
</template>

<script>
  import Bz from './Bz'
  import Doc from 'bz-doc'
  export default {
    components: {
      Bz,
      Doc
    },
    route: {
      deactivate: function (transition) {
        this.$broadcast('unbind-scroll')
        transition.next()
      }
    },
    data: function () {
      return {
        datas: [1],
        name: 'bz-upload-file',
        desc: '上传文件',
        parms: [
          {parm: 'event:change_file', desc: '选不同文件时触发的动作, event 为入参。 e.target.files[0].name 可以取到文件名'},
          {parm: 'event:upload_done', desc: '完成上传后的事件, 上传后的文件地址做为入参'},
          {parm: 'upload_url', desc: "用哪个url来传，默认'/api_file_upload'"},
          {parm: 'accept', desc: '限定文件类型: audio/* video/* image/*'}
        ],
        parm_desc: ``,
        code: `<bz></bz>`
      }
    },
    methods: {
      call_back: function (file_url) {
        window.alert(file_url)
      },
      change_call_back: function (e) {
        window.alert(e.target.files[0].name)
      }
    }
  }
</script>
