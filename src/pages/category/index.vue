<template>
  <div>
    <myheader></myheader>
    <div class="left">
      <scroll-view scroll-y scroll-with-animation>
        <div class="item" v-for="(item,index) in cateList" :key="item.cat_id" @click="selected(index)" :class="{active:index==selectIndex}">
          {{item.cat_name}}
        </div>
      </scroll-view>
    </div>
    <div class="right">
      <scroll-view scroll-y scroll-with-animation>
        <!-- 广告 -->
        <img class="ad-img" src="/static/images/icon/titleImage.png" alt="">
        <!-- 循环对应选中下标的子属性 -->
        <div class="section" v-for="(item, index) in cateList[selectIndex].children" :key="index">
          <!-- 标题 -->
          <h4 class="title">/ <span class="sub">{{item.cat_name}}</span> /</h4>
          <!-- 内容 -->
          <ul class="items">
            <li class="item" v-for="(it, i) in item.children" :key="i">
              <a href="#">
                <img class="pic" :src="'https://autumnfish.cn/wx/'+it.cat_icon" alt="">
                <p class="text">{{it.cat_name}}</p>
              </a>
            </li>
          </ul>
        </div>
      </scroll-view>
    </div>

  </div>
</template>

<script>
  import tool from '../../utils/index'
  import myheader from '../../components/header.vue'
  export default {
    data: function () {
      return {
        cateList: [],
        // 默认选中下标
        selectIndex: 0
      }
    },
    components: {
      myheader
    },
    // 加载分类数据
    onLoad() {
      tool.thenAjax({
          url: 'api/public/v1/categories'
        })
        .then(res => {
          // console.log(res);
          this.cateList = res.data.message;
        })
    },
    methods: {
      // 点击切换类名
      selected(index) {
        // 点击哪个 就是被选中哪个
        this.selectIndex = index;
      }
    }
  }

</script>

<style lang="less">
  page {
    height: 100%;
  }

  page>view {
    height: 100%;
    display: flex;
  }

  .left {
    width: 200rpx;
    height: 100%;

    scroll-view {
      height: 100%;

      .item {
        height: 100rpx;
        box-sizing: border-box;
        text-align: center;
        line-height: 100rpx;
        font-size: 26rpx;
        background-color: #f1f1f1;
        border-bottom: 1px solid gray;

        &.active {
          color: #eb4450;
          position: relative;
          background-color: #fff;

          &::before {
            // 必须给content
            content: "";
            position: absolute;
            width: 8rpx;
            height: 60rpx;
            left: 0;
            top: 20rpx;
            background-color: #eb4450;
          }
        }
      }
    }
  }

  .right {
    flex: 1;
    height: 100%;
    padding: 20rpx 16rpx;

    scroll-view {
      height: 100%;

      .ad-img {
        width: 100%;
        height: 180rpx;
        margin: 0 auto;
        display: block;
      }

      .section {
        text-align: center;

        .title {
          font-size: 26rpx;
          padding: 30rpx 0;
          color: #ccc;

          .sub {
            color: black;
          }
        }

        .items {
          display: flex;
          flex-wrap: wrap;

          .item {
            // flex 1绝对无法换行的 只会均分一行
            width: 33.333%;

            .pic {
              width: 95rpx;
              height: 90rpx;
              display: block;
              margin: 0 auto;
            }

            .text {
              margin-top: 20rpx;
              font-size: 24rpx;
            }
          }
        }
      }
    }
  }

</style>
