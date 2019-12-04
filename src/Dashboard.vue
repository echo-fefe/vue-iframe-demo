<template>
  <div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="Home" name="home">Home</el-tab-pane>
      <el-tab-pane label="Our" name="our">
        <el-row :gutter="20">
          <el-col :span="10">
            <iframe class="iframe" :src="ourUrl" ref="our"></iframe>
          </el-col>
          <el-col :span="6">
            <el-form v-model="formData">
              <el-form-item>
                <el-input class="el-input-width" v-model="formData.text"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button @click="send('data')">向子iframe传数据</el-button>
              </el-form-item>
            </el-form>
          </el-col>
          <el-col>
            从子页面收到的消息：{{ msg }}
          </el-col>
        </el-row>
      </el-tab-pane>
      <el-tab-pane label="Other" name="other">Other</el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeName: 'home',
      formData: {
        text: ''
      },
      msg: '',
      ourUrl: ''
    }
  },
  mounted() {
    window.addEventListener("message", this.receiveMsg, false);
  },
  methods: {
    handleClick(tab, event) {
      // console.log(tab, event)
      if (tab.name === 'our') {
        this.ourUrl = 'http://oriente.kiyola.com/vue-iframe-demo-child/dist/#/'
      } else if (tab.name === 'other') {

      }
    },
    send (name) {
      if (name === 'data') {
        this.$refs['our'].contentWindow.postMessage(this.formData.text, '*')
      }
    },
    receiveMsg (event) {
      const {
        cmd = null,
        params = null
      } = event.data
      console.log(event)
      if (cmd === 'toParent') {
        this.msg = params
      }
    }
  }
}
</script>

<style>
.el-input-width {
  width: 200px;
}

.iframe {
  width: 100%;
}
</style>
