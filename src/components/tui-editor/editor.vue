<!--
 * @Author: wkiwi
 * @Email: w_kiwi@163.com
 * @Date: 2021-09-18 09:34:49
 * @LastEditors: wkiwi
 * @LastEditTime: 2021-09-22 13:47:50
-->
<template>
  <div>
    <div
      id="editor"
      :class="fullScreen ? 'full-screen' : ''"
      style="background:#fff;text-align:left"
    />
  </div>
</template>

<script>
import Editor from '@toast-ui/editor'
import chart from '@toast-ui/editor-plugin-chart/dist/toastui-editor-plugin-chart.js'
import uml from '@toast-ui/editor-plugin-uml/dist/toastui-editor-plugin-uml.js'
import colorSyntax from '@toast-ui/editor-plugin-color-syntax/dist/toastui-editor-plugin-color-syntax.js'
import tableMergedCell from '@toast-ui/editor-plugin-table-merged-cell/dist/toastui-editor-plugin-table-merged-cell.js'
import codeSyntaxHighlight from '@toast-ui/editor-plugin-code-syntax-highlight/dist/toastui-editor-plugin-code-syntax-highlight.js'
import '@toast-ui/editor/dist/i18n/zh-cn.js'
// css
import './tui-color-picker.css'
import 'codemirror/lib/codemirror.css' // Editor's Dependency Style
import '@toast-ui/editor/dist/toastui-editor.css' // Editor's Style

// components
// server
// import { makedownimage } from '@/api/index.js'
export default {
  components: {},
  data() {
    return {
      tuieditor: '',
      showPopup: false,
      fullScreen: false,
    }
  },
  mounted() {
    this.init()
  },
  beforeDestroy() {
    // if (this.tuieditor) {
    //     this.tuieditor.destroy()
    // }
  },
  methods: {
    init() {
      const _that = this
      const tuieditor = new Editor({
        el: document.querySelector('#editor'),
        previewStyle: 'vertical',
        height: '600px',
        initialEditType: 'wysiwyg', // 编辑器的类型，markdown或wysiwyg。
        placeholder: '请输入文字',
        // initialValue: '',
        plugins: [chart, uml, colorSyntax, tableMergedCell, codeSyntaxHighlight],
        // toolbarItems: [
        //     'heading',
        //     'bold',
        //     'italic',
        //     'quote',
        //     'code',
        //     'codeblock',
        //     'table',
        //     'link',
        //     'hr',
        //     'ul',
        //     'ol'
        // ],
        useCommandShortcut: false,
        language: 'zh-CN',
      })
      this.tuieditor = tuieditor
      const toolbar = this.tuieditor.getUI().getToolbar()
      // 全屏幕编辑 full_screen
      toolbar.insertItem(0, {
        type: 'button',
        options: {
          className: 'tui-full_screen',
          event: 'changeFullScreen',
          tooltip: '全屏/初始',
        },
      })
      this.tuieditor.eventManager.addEventType('changeFullScreen')
      this.tuieditor.eventManager.listen('changeFullScreen', function() {
        _that.fullScreen = !_that.fullScreen
      })
      // 语法帮助
      toolbar.insertItem(30, {
        type: 'button',
        options: {
          className: 'tui-Grammar',
          event: 'GrammarCustomButton',
          tooltip: 'Markdown语法帮助',
        },
      })
      this.tuieditor.eventManager.addEventType('GrammarCustomButton')
      this.tuieditor.eventManager.listen('GrammarCustomButton', function() {
        window.open('https://markdown-zh.readthedocs.io/en/latest/')
      })
      // 删除默认监听事件
      this.tuieditor.eventManager.removeEventHandler('addImageBlobHook')
      // 添加自定义监听事件
      this.tuieditor.eventManager.listen('addImageBlobHook', (blob, callback) => {
        // 此处填写自己的上传逻辑，url为上传后的图片地址
        this.addImageBlobHook(blob, callback)
      })

      // this.tuieditor.changeMode('markdown', true)
    },
    addImageBlobHook(blob) {
      if (!['image/gif', 'image/jpeg', 'image/jpg', 'image/png', 'image/svg'].includes(blob.type)) {
        this.$message.error('仅能上传图片')
        return
      }
      const formData = new FormData()
      formData.append('file', blob)
      // makedownimage(formData).then(res => {
      //     if (Number(res.state) !== 1) {
      //         this.$message.error('图片上传错误')
      //         return
      //     }
      //     this.addImgToMd(res.data)
      // })
    },
    // 组件回调的关闭函数
    closeImage() {
      this.showPopup = false
    },
    // 组件回调的关闭函数
    addImageEmit(image) {
      const _that = this
      this.showPopup = false
      const editor = _that.tuieditor.getCodeMirror()
      if (_that.tuieditor.isMarkdownMode()) {
        editor.replaceSelection(`![image](${image})`)
      } else {
        _that.addImgToMd(image)
      }
    },
    // 添加图片到markdown
    addImgToMd(image) {
      const _that = this
      const editorHtml = _that.tuieditor.getCurrentModeEditor()
      const range = editorHtml.getRange()
      const img = document.createElement('img')
      img.src = image
      img.alt = 'image'
      range.insertNode(img)
    },
    getMarkdown() {
      const Markdown = this.tuieditor.getMarkdown()
      this.$emit('getMarkdown', Markdown)
    },
  },
}
</script>

<style scoped lang="scss">
.full-screen {
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  width: 100% !important;
  height: 100% !important;
}
/deep/ .tui-Grammar {
  background: url('./Sprites.png') no-repeat;
  background-position: -7px -33px;
}
/deep/ .tui-tooltip {
  z-index: 9;
}

/deep/ .tui-full_screen {
  background: url('./full_screen.png') no-repeat;
  background-position: -10px -8px;
  &:disabled {
    background-position: -32px -8px;
  }
}
</style>
