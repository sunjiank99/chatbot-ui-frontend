<template>
  <div class="app-container">
    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>卡片名称</span>
          <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>
        </div>
        <!--<div v-for="o in 4" :key="o" class="text item" >-->
        <!--{{'列表内容 ' + o }}-->
        <!--</div>-->

        <!--<div v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">-->
        <!--<div v-for="item in data" :key="item.index">{{ item.name }}</div>-->
        <!--</div>-->
        <!--<el-row>-->
        <!--<ul ref="content" class="infinite-list" style="overflow:auto">-->
        <!--<li v-for="i in count" class="infinite-list-item">-->
        <!--{{ i }}-->
        <!--</li>-->
        <!--<li ref="bottom_chat_item" class="infinite-list-item">加载中</li>-->
        <!--</ul>-->
        <!--</el-row>-->
        <el-row ref="scrollContent" class="infinite-list" style="overflow:auto">
          <template v-for="item in data">
            <el-row v-if="item.formUser===1" class="infinite-list-item">
              <el-col :span="2" align="center">
                <el-avatar :src="robotIcon"/>
              </el-col>
              <div class="bubble-left" />
              <el-col :span="22" style="height: 100%"><div class="chatbot-message">{{ item.content }}</div></el-col>
            </el-row>
            <el-row v-if="item.formUser==0" class="infinite-list-item">
              <el-col :span="22" style="height: 100%"><div class="user-message">{{ item.content }}</div></el-col>
              <div class="bubble-right" />
              <el-col :span="2" align="center">
                <el-avatar icon="el-icon-user-solid" />
              </el-col>
            </el-row>
          </template>
        </el-row>

        <el-row style="margin-top: 10px">
          <el-col :span="20">
            <el-input
              v-model="inputTextArea"
              type="textarea"
              :rows="2"
              placeholder="请输入内容"
            />
          </el-col>
          <el-col :span="4" align="center" style="margin-top: 6px">
            <el-button type="primary" @click="load()">Sent</el-button>
          </el-col>

        </el-row>
      </el-card>
    </el-row>
  </div>

</template>

<script>
import robotIcon from '../../assets/robot-icon.jpg'
export default {
  name: 'StockChatbot',
  data() {
    return {
      count: 10,
      data: [{ content: 'hello', formUser: 0, id: 0 },
        { content: 'hello', formUser: 1, id: 1 },
        { content: 'hello', formUser: 0, id: 2 }],
      busy: false,
      inputTextArea: '',
      robotIcon: robotIcon
    }
  },
  watch: {
    'data': function() {
    //   Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。
    //   如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。
    //   然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。
    //   因此用nextTick保持滚动条始终触底
      this.$nextTick(() => {
        this.$refs.scrollContent.$el.scrollTop = (this.$refs.scrollContent.$el.scrollHeight)
      })
    }
  },
  methods: {
    load() {
      var item = { content: 'hello', formUser: 0, id: 0 }
      if (this.inputTextArea === '') {
        item.content = '  '
      } else {
        item.content = this.inputTextArea
      }
      this.data.push(item)
      // this.$refs.content.scrollTop = (this.$refs.bottom_chat_item.offsetTop + 100)
      // this.$refs.content.scrollTop = (this.$refs.content.scrollHeight + 100)

      // this.$nextTick(() => {
      //   this.$refs.content.scrollTop = (this.$refs.content.scrollHeight)
      // })
      console.log(this.$refs.scrollContent.$el.scrollHeight)
    }
  }
}

</script>

<style scoped>
  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }

  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 600px;
  }

  .infinite-list {
    height: 400px;
    padding: 0;
    margin: 0;
    list-style: none;
  }

  .infinite-list-item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    background: #ffffff;
    margin: 10px;
  }

  .bubble-left {
    width: 0px;
    height: 0px;
    border: 10px solid;
    border-color: transparent skyblue transparent transparent;
    content: '';
  }
  .bubble-right {
    width: 0px;
    height: 0px;
    border: 10px solid;
    border-color: transparent transparent transparent skyblue;
    content: '';
  }
  .chatbot-message{
    background-color:skyblue;
    height: 100% ;
    border-radius:4px;
    width: fit-content;
  }
  .user-message{
    background-color:skyblue;
    height: 100% ;
    border-radius:4px;
    width: fit-content;
    float:right;
  }

</style>
