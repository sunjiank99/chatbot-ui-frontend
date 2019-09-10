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
        <ul ref="content" class="infinite-list" style="overflow:auto">
          <li v-for="i in count" class="infinite-list-item">{{ i }}</li>
          <li ref="bottom_chat_item" class="infinite-list-item">加载中</li>
        </ul>
        <button @click="load()">test</button>
      </el-card>
    </el-row>
  </div>

</template>

<script>
export default {
  name: 'StockChatbot',
  data() {
    return {
      count: 10,
      data: [],
      busy: false
    }
  },
  watch: {
    'count': function() {
      // Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。
      // 如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。
      // 然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。
      // 因此用nextTick保持滚动条始终触底
      this.$nextTick(() => {
        this.$refs.content.scrollTop = (this.$refs.content.scrollHeight)
      })
    }
  },
  methods: {
    loadMore: function() {
      this.busy = true
      setTimeout(() => {
        for (var i = 0, j = 10; i < j; i++) {
          this.data.push({ name: this.count++ })
        }
        console.log(this.data)
        this.busy = false
      }, 1000)
    },
    load() {
      this.count += 2
      // this.$refs.content.scrollTop = (this.$refs.bottom_chat_item.offsetTop + 100)
      // this.$refs.content.scrollTop = (this.$refs.content.scrollHeight + 100)
      console.log(this.$refs.content.lastChild.offsetTop)
    }
  }
}

</script>

<style scoped>
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 480px;
  }

  .infinite-list {
    height: 300px;
    padding: 0;
    margin: 0;
    list-style: none;
  }
  .infinite-list-item {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    background: #e8f3fe;
    margin: 10px;
    color: #7dbcfc;
  }
</style>
