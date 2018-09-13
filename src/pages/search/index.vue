<template>
  <div>
    <!-- 顶部的搜索框 -->
    <div class="search-box">
      <icon type="search" class="search" size="20">
      </icon>
      <input class="input" v-model="search" @confirm="submit" type="text" placeholder="请输入你想要的商品">
      <input class="button" type="button" @click="cancel" value="清空">
    </div>
    <!-- 历史记录 -->
    <div class="history-box">
      <div class="control">
        <span>历史搜索</span>
        <span @click="clear" class="iconfont icon-shanchu"></span>
      </div>
      <ul class="items">
        <li v-for="(item, index) in history" :key="index" class="item">{{item}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        search: '',
        // 搜索的历史记录
        history: []
      }
    },
    onLoad() {
      // 第一次加载页面就去查找缓存中的数据 有则取值渲染 
      wx.getStorage({
        key: 'history',
        success: res => {
          this.history = res.data
        },
        fail: res => {
          console.log(res)
        },
        complete: () => {}
      });
    },
    methods: {
      // 搜索事件
      submit(event) {
        // 获取Storage数据
        // 去重
        // 用indexOf方法判断 如果为不为 表示有相同数据 直接删除之前数据
        let index = this.history.indexOf(this.search);
        if (index != -1) {
          this.history.splice(index, 1);
        }
        // 不为-1把这个搜索的内容追加到数组中 并添加到storage中 缓存数据   
        this.history.push(this.search);
        wx.setStorage({
          key: 'history',
          data: this.history
        });
      },
      //清空输入内容
      cancel() {
        this.search = '';
      },
      // 清空历史纪录
      clear() {
        // 情空页面数据
        this.history = [];
        //清空 缓存数据
        wx.setStorage({
          key: 'history',
          data: this.history
        });
      }
    }
  }

</script>


<style lang='less'>
  page {
    padding-top: 0;
  }

  .search-box {
    position: relative;
    height: 120rpx;
    background-color: #eeeeee;
    display: flex;
    padding: 0 16rpx;
    align-items: center;

    .search {
      left: 35rpx;
      position: absolute;
      top: 40rpx;
      z-index: 998;
    }

    .input {
      flex: 1;
      font-size: 26rpx;
      color: #808080;
      margin-right: 20rpx;
      background-color: white;
      padding-left: 70rpx;
      border-radius: 6rpx;
      height: 60rpx;
    }

    .button {
      width: 160rpx;
      border: 1px solid #BFBFBF;
      font-size: 30rpx;
      line-height: 60rpx;
      height: 60rpx;
    }
  }

  // 历史记录
  .history-box {
    .control {
      height: 80rpx;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 32rpx;
      padding: 0 20rpx;

      .iconfont {
        width: 30rpx;
        height: 30rpx;
        color: #CCCCCC;
      }
    }

    .items {
      display: flex;
      flex-wrap: wrap;
      padding-left: 16rpx;

      .item {
        height: 64rpx;
        background-color: #DDDDDD;
        font-size: 26rpx;
        line-height: 64rpx;
        padding: 0 20rpx;
        margin-right: 30rpx;
        margin-top: 16rpx;
      }
    }
  }

</style>
