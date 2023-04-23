<template>
  <div class="message" style="height: 95%">
    <!--    <el-dialog :visible.sync="dialogVisible">-->
    <!--&lt;!&ndash;      <user-login></user-login>&ndash;&gt;-->
    <!--      <span>这是一段信息</span>-->
    <!--      <span slot="footer" class="dialog-footer">-->
    <!--    <el-button @click="dialogVisible = false">取 消</el-button>-->
    <!--    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>-->
    <!--  </span>-->
    <!--    </el-dialog>-->

    <el-row style="transition: height 1s;" :style="{ height: computedHeight1 }"></el-row>
    <el-row type="flex" justify="center">
      <el-col :span="6"
              style="color:white;font-size: 40px;display: flex;justify-content: flex-end;align-items: center;">数智医疗
      </el-col>
      <el-col :span="2"><img src="src/assets/doctor.png" width="106" height="115" alt="logo"></el-col>
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
import {nextTick} from "vue";
import axios from 'axios';

export default {
  components: {UserLogin},
  data() {
    return {
      messages: [],
      content: '',
      dialogVisible: true,
    }
  },
  methods: {
    onKeyup(e) {
      if (e.keyCode === 13 && this.content.length) {
        this.sendMessage(this.content);

      }
    },
    sendMessage(message) {
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
        // console.log(response)
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
  },
  computed: {
    messageEmpty() {
      return this.messages.length === 0
    },
    computedHeight1() {
      return this.messageEmpty ? '30%' : '0%';
    },
    computedHeight2() {
      return this.messageEmpty ? '0%' : '75%';
    },
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