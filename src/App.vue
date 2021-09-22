<!--
 * @Author: wkiwi
 * @Email: w_kiwi@163.com
 * @Date: 2021-09-22 11:27:56
 * @LastEditors: wkiwi
 * @LastEditTime: 2021-09-22 13:52:49
-->
<template>
  <div id="app">
    <tuiEditor
      ref="editor"
      @getMarkdown="getMarkdown"/>
      <!-- 编辑器 -->
      <button @click="save">获取markdown代码</button>

      <tuiView :content="content"/>
      <!-- 显示器 -->
  </div>
</template>

<script>
import tuiEditor from '@/components/tui-editor/editor.vue'
import tuiView from '@/components/tui-editor/view.vue'
export default {
  name: 'App',
  components: {
    tuiEditor,
    tuiView
  },
  data(){
    return {
      content:` # 测试

## 测试`
    }
  },
  methods:{
    /**
     * @desc: 仅保存  不校验数据完整性
     * @Author: wkiwi
     * @function: save
     */
    save() {
        this.$refs.editor.getMarkdown()
    },
    getMarkdown(mdContent) {
        console.log(mdContent)
        this.mdContent = mdContent
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
