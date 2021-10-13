<template>
  <v-app>
    <v-main>
      <v-container>
        <v-alert
            v-if="showAlert"
            dense
            :type="alertType"
        >{{ alertMessage }}
        </v-alert>
      </v-container>
      <v-container
          v-for='(item, n) in articles'
          :key='n'
      >
        <v-card elevation="5" outlined shaped>
          <v-card-title>{{ item.author }}</v-card-title>
          <v-card-subtitle>{{ item.qq }}</v-card-subtitle>
          <v-container>
            <div v-html='myRender(item.content)'></div>
          </v-container>
        </v-card>

      </v-container>

      <v-container>
        <div style="text-align: center;color: deepskyblue">尝试写文章？</div>

        <br>

        <mavon-editor
            ref="md"
            @change="change"
            :toolbars="toolbars"
            :value="inputValue"
        ></mavon-editor>

        <v-form v-model="valid" ref="form">
          <v-container>
            <v-row>
              <v-text-field
                  v-model="name"
                  :rules="nameRules"
                  :counter="10"
                  label="告诉我你是谁"
                  required
              ></v-text-field>
              <v-spacer></v-spacer>
              <v-text-field
                  v-model="qq"
                  :rules="qqRules"
                  label="告诉我你的QQ"
                  required
              ></v-text-field>
            </v-row>
            <br>
            <v-btn
                block
                @click="submit"
                elevation="7"
                color="pink"
                style="color: white"
                :disabled="!valid"
            >
              提交
              <v-icon right dark>mdi-cloud-upload</v-icon>
            </v-btn>
          </v-container>

        </v-form>

      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import 'highlight.js/scss/default.scss'
import mavonEditor from "mavon-editor";

let md = mavonEditor.markdownIt

export default {
  name: 'App',

  data: () => ({
    //alert
    showAlert: false,
    alertType: '',
    alertMessage: '',

    //form
    valid: false,
    name: '',
    nameRules: [
      v => !!v || '请填写名字',
      v => v.length <= 10 || '名字太长啦，应该小于10个字符',
    ],
    qq: '',
    qqRules: [
      v => !!v || '必须填写QQ号',
      v => (v.length <= 10 && v.length >= 8 && v.match(/^[0-9]+$/) != null) || '请输入真的QQ号'
    ],

    inputValue: "",
    articles: [],
    toolbars: {
      bold: true, // 粗体
      italic: true, // 斜体
      header: true, // 标题
      underline: true, // 下划线
      strikethrough: true, // 中划线
      mark: true, // 标记
      superscript: true, // 上角标
      subscript: true, // 下角标
      quote: true, // 引用
      ol: true, // 有序列表
      ul: true, // 无序列表
      link: true, // 链接
      imagelink: false, // 图片链接
      code: true, // code
      table: true, // 表格
      fullscreen: true, // 全屏编辑
      readmodel: true, // 沉浸式阅读
      htmlcode: true, // 展示html源码
      help: true, // 帮助
      undo: true, // 上一步
      redo: true, // 下一步
      trash: true, // 清空
      save: true, // 保存（触发events中的save事件）
      navigation: true, // 导航目录
      alignleft: true, // 左对齐
      aligncenter: true, // 居中
      alignright: true, // 右对齐
      subfield: true, // 单双栏模式
      preview: true, // 预览
    },
  }),
  methods: {
    getArticles() {
      const path = 'http://119.34.0.52:5050/articles'
      axios.get(path).then(
          (res) => {
            this.articles = res.data['articles']
          }
      ).catch((error) => {
        this.alert('error', 'network error!')
        console.error(error)
      })
    },
    alert(type, message) {
      this.showAlert = true
      this.alertType = type
      this.alertMessage = message
    },
    change(value) {
      this.inputValue = value
    },
    myRender(text) {
      return md.render(text)
    },
    submit() {
      const path = 'http://10.30.106.70:5000/articles';
      let payload = {
        author: this.name,
        content: this.inputValue,
        qq: this.qq,
      }
      axios.post(path, payload)
          .then(() => {
            this.alert('success', 'Article successfully submitted');
            this.getArticles();
          })
          .catch((error) => {
            // eslint-disable-next-line
            this.alert('error', 'failed to add an article: ' + error)
            this.getArticles();
          });
      this.inputValue = ''
      this.$refs.form.reset()
    }
  },
  created() {
    this.getArticles()
  }
};
</script>
