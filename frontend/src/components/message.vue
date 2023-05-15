<template>
  <div class="message" style="height: 95%">
    <el-dialog
        v-model="dialogVisible"
        width="30%"
    >
      <user-login v-if="!hasLogin" v-on:login-success="loginSuccess"></user-login>
      <div v-else>
        <div>你好，{{username}}</div>
      </div>
      <template #footer>
      <span class="dialog-footer">
        <el-button type="danger" v-if="hasLogin" @click="hasLogin = false">退出</el-button>
        <el-button @click="dialogVisible = false">取消</el-button>
      </span>
      </template>
    </el-dialog>

    <el-row style="transition: height 1s;" :style="{ height: computedHeight1 }" type="flex" justify="center">
      <el-col style="display: flex;justify-content: flex-end;">
        <div style="color:white">{{shownUsername}}</div>
        <el-icon size="30" color="white" @click="dialogVisible = true"><User /></el-icon>
      </el-col>

    </el-row>
    <el-row type="flex" justify="center">
      <el-col :span="6"
              style="color:white;font-size: 40px;display: flex;justify-content: flex-end;align-items: center;">数智医疗
      </el-col>
      <el-col :span="2"><img src="src/assets/doctor.png" width="140" height="150" alt="logo"></el-col>
      <el-col :span="6" style="color:white;font-size: 40px;display: flex;align-items: center;">AI私助</el-col>
    </el-row>

    <el-row id="history" style="transition: height 1s; overflow-y: auto;" :style="{ height: computedHeight2}">
      <ul style="width: 95%">
        <li v-for="item in messages">
          <div class="main" :class="{ self: item.self }">
            <div class="text">{{ item.content }}</div>
          </div>
        </li>
      </ul>
    </el-row>

    <el-row type="flex" justify="center">
      <el-col :span="messageEmpty?10:20">
        <el-input v-model="content" placeholder="按 Enter 发送" @keyup="onKeyup"/>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import UserLogin from "./UserLogin.vue";
import { ElNotification } from 'element-plus';
import {nextTick} from "vue";
import axios from 'axios';

export default {
  components: {UserLogin},
  data() {
    return {
      messages: [],
      content: '',
      dialogVisible: false,
      hasLogin: false,
      token: '',
      username: '',
    }
  },
  methods: {
    onKeyup(e) {
      if (e.keyCode === 13 && this.content.length) {
        this.sendMessage(this.content);

      }
    },
    sendMessage(message) {
      if(!this.hasLogin){
        this.messages.push({
          content: '请先登录'
        })
        return
      }
      this.messages.push({
        content: message,
        self: true
      })
      this.content = '';
      nextTick(() => {
        this.moveBottom()
      })
      this.replyMessage(message)
    },
    replyMessage(message) {
      let param = new URLSearchParams()
      param.append('user', message)
      axios({
        method: 'post',
        url: '/api/chat2',
        data: param
      }).then(response => {
        this.messages.push({
          content: response['data']['ans']
        })
      }).catch(error => {
        console.log(error)
      });

      nextTick(() => {
        this.moveBottom()
      })
    },
    moveBottom() {
      const element = document.getElementById('history')
      element.scrollTop = element.scrollHeight
    },
    loginSuccess(userInfo) {
      this.hasLogin = true
      this.token = userInfo.token
      this.username = userInfo.username
      this.dialogVisible = false
      ElNotification({
        title: '登陆成功',
        message: '你好，'+this.username,
        type: 'success',
      })
    }
  },
  computed: {
    messageEmpty() {
      return this.messages.length === 0
    },
    computedHeight1() {
      return this.messageEmpty ? '30%' : '6%';
    },
    computedHeight2() {
      return this.messageEmpty ? '0%' : '74%';
    },
    shownUsername() {
      return this.hasLogin ? this.username : '未登录'
    }
  },
}
</script>


<style lang="less" scoped>
.message {
  padding: 10px 15px;
  background-color: #422C8E;
  width: 100%;
  height: 100%;

  li {
    margin-bottom: 15px;
  }

  .text {
    display: inline-block;
    position: relative;
    padding: 0 10px;
    max-width: ~'calc(100% - 40px)';
    min-height: 30px;
    line-height: 2.5;
    font-size: 12px;
    text-align: left;
    word-break: break-all;
    background-color: #fafafa;
    border-radius: 4px;

    &:before {
      content: " ";
      position: absolute;
      top: 9px;
      right: 100%;
      border: 6px solid transparent;
      border-right-color: #fafafa;
    }

  }

  .self {
    text-align: right;

    .text {
      background-color: #DEF0FA;

      &:before {
        right: inherit;
        left: 100%;
        border-right-color: transparent;
        border-left-color: #DEF0FA;
      }
    }
  }
}

</style>