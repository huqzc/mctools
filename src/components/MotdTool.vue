<template>
  <div style="height: 100%">
    <el-container>
      <el-header>
        <template v-for="(item, index) in dict">
          <el-button @click="addColor(index)" :style="{backgroundColor: index}"></el-button>
        </template>
      </el-header>
      <el-main>
        <div contenteditable="true" ref="editor" class="textarea">
          <span style="white-space: pre" v-html="textarea"></span>
        </div>
        <el-input v-model="codeString" ref="input"></el-input>
      </el-main>
      <el-footer>
        <el-button @click="genCode">复制</el-button>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
  export default {
    name: "MotdTool",
    data() {
      return {
        textarea: 'What a colorful motd!',
        dict: {
          "darkred": "\\u00A74",
          "red": "\\u00A7c",
          "gold": "\\u00A76",
          "yellow": "\\u00A7e",
          "darkgreen": "\\u00A72",
          "green": "\\u00A7a",
          "aqua": "\\u00A7b",
          "#05696B": "\\u00A73",
          "darkblue": "\\u00A71",
          "blue": "\\u00A79",
          "mediumpurple": "\\u00A7d",
          "purple": "\\u00A75",
          "white": "\\u00A7f",
          "gray": "\\u00A77",
          "darkgray": "\\u00A78",
          "black": "\\u00A70"
        },
        codeString: ''
      }
    },
    methods: {
      addColor(color) {
        let s = document.getSelection().toString()
        if (!s) return
        this.handle('insertHTML', '<span style="color: ' + color + ';">' + s + '</span>')
        this.textarea = this.$refs.editor.innerHTML
      },
      handle(name, args) {
        document.execCommand(name, null, args || null)
      },
      genCode() {
        let data = this.textarea
        let re = RegExp('<span style="color: (.*?);">(.*?)</span>', 'm')
        while(re.test(data)) {
          data = data.replace(re, this.dict[RegExp.$1] + RegExp.$2)
        }
        data = data.replace(/<span .*?>(.*?)<\/span>/gm, '$1').replace(/<.*?>/gm, '')
        this.copy(data)
      },
      copy(data) {
        this.codeString = data
        this.$nextTick(() => {
          this.$refs.input.select()
          setTimeout(() => {
            this.handle('copy')
            this.$message.success('复制成功')
          }, 300)
        })
      }
    }
  }
</script>

<style scoped>

  .el-container {
    height: 100%;
    text-align: center;
  }

  .el-main {
    overflow: hidden;

  }

  .textarea {
    height: 200px;
    width: 100%;
    border: 1px solid #cccccc;
    border-radius: 10px;
    outline: none;
    font-family: 'minecraft';
    font-size: 22px;
    display: flex;
    align-content: center;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    word-break: break-all;
    background-image: url("@/assets/bg.png");
    color: white;
  }
</style>
