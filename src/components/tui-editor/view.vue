<!--
 * @Author: wkiwi
 * @Email: w_kiwi@163.com
 * @Date: 2021-09-18 11:27:49
 * @LastEditors: wkiwi
 * @LastEditTime: 2021-09-22 13:47:31
-->
<template>
  <div id="viewer" />
</template>

<script>
import Viewer from '@toast-ui/editor/dist/toastui-editor-viewer'
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
export default {
  props: {
    content: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      viewer: '',
    }
  },
  watch: {
    content(newVal) {
      if (newVal) {
        this.viewer.setMarkdown(this.content)
      }
    },
  },
  mounted() {
    const viewer = new Viewer({
      el: document.querySelector('#viewer'),
      // viewer: true,
      // height: '100%', //
      initialValue: this.content, // 初始值，设置Markdown字符串。
      plugins: [chart, uml, tableMergedCell, colorSyntax, codeSyntaxHighlight],
    })
    this.viewer = viewer
  },
}
</script>

<style></style>
