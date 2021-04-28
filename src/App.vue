<template>
  <div>
   <section>
     {{msg}}
     <tinymce-editor ref="editor"
                     v-model="msg"
                     :disabled="disabled"
                     @onClick="onClick">
     </tinymce-editor>
     <button @click="clear">清空内容</button>
     <button @click="disabled = true">禁用</button>
   </section>
    <section>
      <howler></howler>
    </section>
  </div>
</template>

<script>

  import TinymceEditor from './components/Edit'
  import howler from './components/howler'
  export default {
    components: {
      TinymceEditor,
      howler
    },
    data () {
      return {
        msg: 'Welcome to Use Tinymce Editor',
        disabled: false
      }
    },
    methods: {
      // 鼠标单击的事件
      onClick (e, editor) {
        console.log('Element clicked')
        console.log(e)
        console.log(editor)
      },
      // 清空内容
      clear () {
        this.$refs.editor.clear()
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
