# Vue 2.X 版本+tinymce 5富文本编辑器整合

## 安装tinymce-vue ，必须为3.X版本，不然会报错
```
npm install @tinymce/tinymce-vue@3.2.8 -S

```

### 安装tinymce
```
npm install tinymce -S

```

### 下载中文语言包
tinymce提供了很多的语言包，这里我们下载中文语言包
See [中文语言包](https://www.tiny.cloud/get-tiny/language-packages/).

## 使用
1、在public目录下新建tinymce，将上面下载的语言包解压到该目录
2、在node_modules里面找到tinymce,将skins目录复制到public/tinymce里面
  最终形成以下目录形式：
  
 
  ![avatar](https://image.liubing.me/2019/12/26/a5b859754b616.png)
  
  
### 封装组件
```
<template>
<div class="tinymce-editor">
  <editor v-model="myValue"
          :init="init"
          :disabled="disabled"
          @onClick="onClick">
  </editor>
</div>
</template>
<script>
  import tinymce from 'tinymce/tinymce'
  import Editor from '@tinymce/tinymce-vue'
  import 'tinymce/themes/silver'
  import 'tinymce/icons/default/icons.min.js'
  // 编辑器插件plugins
  // 更多插件参考：https://www.tiny.cloud/docs/plugins/
  import 'tinymce/plugins/image'// 插入上传图片插件
  import 'tinymce/plugins/media'// 插入视频插件
  import 'tinymce/plugins/table'// 插入表格插件
  import 'tinymce/plugins/lists'// 列表插件
  import 'tinymce/plugins/wordcount'// 字数统计插件
  export default {
    components: {
      Editor
    },
    props: {
      value: {
        type: String,
        default: ''
      },
      // 基本路径，默认为空根目录，如果你的项目发布后的地址为目录形式，
      // 即abc.com/tinymce，baseUrl需要配置成tinymce，不然发布后资源会找不到
      baseUrl: {
        type: String,
        default: ''
      },
      disabled: {
        type: Boolean,
        default: false
      },
      plugins: {
        type: [String, Array],
        default: 'lists image media table wordcount'
      },
      toolbar: {
        type: [String, Array],
        default: 'undo redo |  formatselect | bold italic forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat'
      }
    },
    data () {
      return {
        init: {
          language_url: `${this.baseUrl}/tinymce/langs/zh_CN.js`,
          language: 'zh_CN',
          skin_url: `${this.baseUrl}/tinymce/skins/ui/oxide`,
          content_css: `${this.baseUrl}/tinymce/skins/content/default/content.css`,
          // skin_url: `${this.baseUrl}/tinymce/skins/ui/oxide-dark`, // 暗色系
          // content_css: `${this.baseUrl}/tinymce/skins/content/dark/content.css`, // 暗色系
          height: 300,
          plugins: this.plugins,
          toolbar: this.toolbar,
          branding: false,
          menubar: false,
          // 此处为图片上传处理函数，这个直接用了base64的图片形式上传图片，
          // 如需ajax上传可参考https://www.tiny.cloud/docs/configure/file-image-upload/#images_upload_handler
          images_upload_handler: (blobInfo, success) => {
            const img = 'data:image/jpeg;base64,' + blobInfo.base64()
            success(img)
          }
        },
        myValue: this.value
      }
    },
    mounted () {
      tinymce.init({})
    },
    methods: {
      // 添加相关的事件，可用的事件参照文档=> https://github.com/tinymce/tinymce-vue => All available events
      // 需要什么事件可以自己增加
      onClick (e) {
        this.$emit('onClick', e, tinymce)
      },
      // 可以添加一些自己的自定义事件，如清空内容
      clear () {
        this.myValue = ''
      }
    },
    watch: {
      value (newValue) {
        this.myValue = newValue
      },
      myValue (newValue) {
        this.$emit('input', newValue)
      }
    }
  }
</script>
```

### 使用组件
```javascript
<template>
  <div>
    {{msg}}
    <tinymce-editor ref="editor"
                    v-model="msg"
                    :disabled="disabled"
                    @onClick="onClick">
    </tinymce-editor>
    <button @click="clear">清空内容</button>
    <button @click="disabled = true">禁用</button>
  </div>
</template>

<script>

  import TinymceEditor from './components/HelloWorld' // 上面封装的组件
  export default {
    components: {
      TinymceEditor
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

```
### 效果图
  ![avatar](https://image.liubing.me/2019/12/26/a74de0869dd5e.gif)
