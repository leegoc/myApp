<template>
  <div>
    <div class="editor-container">
      <div id="editor-toolbar" class="toolbar"></div>
      <div id="editor-text"></div>
    </div>
  </div>
</template>

<script>
import E from 'wangeditor'
export default {
  name: 'index',
  data () {
    return {
      editor: '',
    }
  },
  mounted() {
    this.initEditor()
  },
  methods: {
    
    initEditor() {
      this.editor = new E('#editor-toolbar', '#editor-text')
      this.editor.customConfig.menus = [
        'head',  // 标题
        'bold',  // 粗体
        'fontSize',  // 字号
        'fontName',  // 字体
        'italic',  // 斜体
        'underline',  // 下划线
        'strikeThrough',  // 删除线
        'foreColor',  // 文字颜色
        'backColor',  // 背景颜色
        'link',  // 插入链接
        'list',  // 列表
        'justify',  // 对齐方式
        'quote',  // 引用
        // 'emoticon',  // 表情
        'image',  // 插入图片
        'table',  // 表格
        'video',  // 插入视频
        // 'code',  // 插入代码
        'undo',  // 撤销
        'redo'  // 重复
      ]
      this.editor.customConfig.uploadImgShowBase64 = true   // 使用 base64 保存图片
      this.editor.create()
    },
  }
}
</script>

<style scoped>
.editor-container {
  min-width: 968px;
  width: 80vw;
  height: 80vh;
}

#editor-text {
  width: 100%;
  border: 1px solid #333;
}

.toolbar {
  height: 30px;
  border: 1px solid #333;
  border-bottom: none;
}

#editor-text {
  height: 700px;
}

#text-elem8361085167523137 > * {
  width: 100%;
}

</style>
