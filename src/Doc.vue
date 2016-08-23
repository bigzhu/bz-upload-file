<style lang=less>
</style>

<template>
  <div>
    <doc :name="name"
      :desc="desc"
      :parm_desc="parm_desc"
      :parms="parms"
      :code="code"
      >
      <bz :change_call_back="change_call_back" accept="image/*" :done_call_back="call_back"></bz>
    </doc>
  </div>
</template>

<script>
  import 'bz-semantic-ui-card'
  import 'bz-semantic-ui-grid'
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
          {parm: 'change_call_back', desc: '选不同文件时触发的动作, event 为入参'},
          {parm: 'done_call_back', desc: '完成上传后的回调函数, 上传后的文件地址做为入参'},
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
