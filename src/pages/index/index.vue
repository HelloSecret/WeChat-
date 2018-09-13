<template>
  <div>
    <!-- 搜索栏 -->
    <!-- <div class="header">
      <icon type="search" size="16"></icon>
      <input type="text" placeholder="搜索">
    </div> -->
        <myheader></myheader>
    <swiper indicator-dots autoplay circular>
      <swiper-item v-for="item in swiperList" :key="item.name">
        <image mode="aspectFill" :src="item.image_src"></image>
      </swiper-item>
    </swiper>
    <!-- 分类 -->
    <ul class="cate-box">
      <li v-for="(item, index) in cateList" :key="index" class="item">
        <img class="img" :src="item.image_src" alt="">
        <text>{{item.name}}</text>
      </li>
    </ul>
    <!-- 楼层 -->
    <div class="section" v-for="(item, index) in floorList" :key="index">
      <div class="title">
        <text class="text">{{item.floor_title.name}}</text>
        <img class="image" :src="item.floor_title.image_src" alt="">
      </div>
      <div class="content">
        <a href="#" class="nav" v-for="(it, i) in item.product_list" :key="i">
          <img class="image" :src="it.image_src" alt="">
        </a>
      </div>
    </div>
    <!-- 底线 -->
    <div class="footer">
      <span class="iconfont icon-xiao"></span>我是有底线的!
    </div>
    <!-- 回到顶部 -->
    <div @click="toTop" v-show="isShow" class="to-top">
      <span class="iconfont icon-jiantoushang"></span>
      顶部
    </div>
  </div>
</template>

<script>
  // 引入封装的ajax函数
  import tool from '../../utils/index'
  import myheader from '../../components/header.vue'
  export default {
    data: function () {
      return {
        swiperList: [], //轮播数据
        cateList: [], //分类数据
        floorList: [], //楼层数据
        isShow: false
      }
    },
    // 生命周期函数
    onLoad() {
      tool.thenAjax({
          url: 'api/public/v1/home/swiperdata'
        })
        //  轮播图
        .then(res => {
          // console.log(res);
          this.swiperList = res.data.message
          return tool.thenAjax({
            url: 'api/public/v1/home/catitems'
          })
        })
        // 分类数据
        .then(res => {
          // console.log(res);
          this.cateList = res.data.message;
          return tool.thenAjax({
            url: 'api/public/v1/home/floordata'
          })
        })
        .then(res => {
          // console.log(res);
          this.floorList = res.data.message;
        })
    },
    onPageScroll(e) {
      // console.log(e);
      if (e.scrollTop > 192) {
        this.isShow = true;
      } else {
        this.isShow = false;
      }
    },
    // 自己绑定的事件
    methods: {
      toTop() {
        // 微信原生回到顶部
        wx.pageScrollTo({
          scrollTop: 0,
          duration: 300
        });
      }
    },
    components:{
        myheader
    }
  };

</script>

<style lang='less'>
  // .header {
  //   padding: 20rpx 16rpx;
  //   position: relative;
  //   background-color: #FF2D4A;

  //   icon {
  //     position: absolute;
  //     top: 34rpx;
  //     left: 50%;
  //     transform: translateX(-200%);
  //     z-index: 998;
  //   }

  //   input {
  //     height: 60rpx;
  //     border-radius: 10rpx;
  //     background-color: white;
  //     width: 100%;
  //     color: #bbb;
  //     text-align: center;
  //     font-size: 24rpx;
  //   }
  // }

  swiper {
    height: 384rpx;

    swiper-item {
      image {
        display: block;
        width: 100%;
        height: 100%;
      }
    }
  }

  .cate-box {
    display: flex;
    padding-bottom: 30rpx;
    padding-top: 24rpx;

    .item {
      flex: 1;

      .img {
        flex: 1;
        display: block;
        width: 100rpx;
        height: 100rpx;
        margin: 0 auto 20rpx;
      }

      text {
        font-size: 24rpx;
        display: block;
        text-align: center;
      }
    }
  }

  // 楼层数据
  .section {
    .title {
      position: relative;

      .text {
        position: absolute;
        color: #ff7b94;
        font-weight: 700;
        font-size: 50rpx;
        left: 20rpx;
        top: 10rpx;
      }

      .image {
        height: 85rpx;
        width: 100%;
      }
    }

    .content {
      padding: 20rpx 16rpx;
      height: 440rpx;

      .nav {
        display: block;
        float: left;
        width: 33.333%;
        height: 100%;
        padding: 5rpx;
        box-sizing: border-box;
      }

      .image {
        display: block;
        width: 100%;
        height: 100%;
      }

      // 只要不是第一个 nav
      .nav:not(:first-child) {
        height: 50%;
      }
    }
  }

  // 底线布局
  .footer {
    background-color: #f4f4f4;
    color: #999;
    font-size: 24rpx;
    text-align: center;
    height: 140rpx;
    line-height: 140rpx;

    .iconfont {}
  }

  .to-top {
    width: 80rpx;
    height: 80rpx;
    text-align: center;
    font-size: 28rpx;
    color: #737373;
    background-color: #eee;
    position: fixed;
    bottom: 50rpx;
    right: 20rpx;
    border-radius: 50%;

    .iconfont {
      display: block;
    }
  }

</style>
