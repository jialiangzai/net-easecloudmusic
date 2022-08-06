<template>
  <view class="index">
    <musichead title="有音乐来听歌" :icon="false"></musichead>
    <view class="container">
      <scroll-view scroll-y="true">
        <view class="index-search" @tap="handleToSearch">
          <text class="iconfont iconsearch"></text>
          <input type="text" placeholder="搜索歌曲" />
        </view>
        <view v-if="isLoading">
          <m-for-skeleton :avatarSize="200" :row="3" :loading="isLoading" isarc="square" v-for="(item,key) in 4"
            :key="key" :titleStyle="{}">
          </m-for-skeleton>
        </view>
        <view class="index-list" v-else>
          <view class="index-list-item" v-for="(item,index) in topList" :key="item.id" @tap="handleToList(item.id)">
            <view class="index-list-img">
              <image :src="item.coverImgUrl" mode=""></image>
              <text>{{item.updateFrequency}}</text>
            </view>
            <view class="index-list-text">
              <view v-for="(musicItem,index) in item.tracks" :key="index" class="twe">
                {{index+1}}.{{musicItem.first}}.{{musicItem.second}}
              </view>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
  // 导入组件
  import mForSkeleton from "@/components/m-for-skeleton/m-for-skeleton";
  import '@/common/iconfont.css'
  import {
    topList
  } from '@/common/api'
  export default {
    data() {
      return {
        title: 'Hello',
        topList: [],
        isLoading: true
      }
    },
    onLoad() {
      this.getTopList()
    },
    methods: {
      handleToSearch() {
        uni.navigateTo({
          url: '/pages/search/search'
        });
      },
      // 跳转到列表页
      handleToList(listId) {
        uni.navigateTo({
          url: '/pages/list/list?listId=' + listId
        })
      },
      // 获取首页摘要
      async getTopList() {
        let res = await topList()
        if (res.length > 0) {
          this.topList = res;
          setTimeout(()=>{
          this.isLoading = false
          },1600)
        }
      }
    }
  }
</script>

<style>
  .index {}

  .index-search {
    display: flex;
    /* 上下居中 */
    align-items: center;
    height: 70rpx;
    margin: 0 30rpx 30rpx 30rpx;
    background: #f7f7f7;
    border-radius: 50rpx;
  }

  .index-search text {
    font-size: 26rpx;
    margin-right: 26rpx;
    margin-left: 28rpx;
  }

  .index-search input {
    font-size: 28rpx;
    flex: 1;
  }

  .index-list {
    margin: 0 30rpx;
  }

  .index-list-item {
    display: flex;
    margin-bottom: 34rpx;
  }

  .index-list-img {
    min-width: 212rpx;
    height: 212rpx;
    position: relative;
    border-radius: 30rpx;
    overflow: hidden;
    margin-right: 22rpx;
  }

  .index-list-img image {
    width: 100%;
    height: 100%;
  }

  .index-list-img text {
    position: absolute;
    left: 12rpx;
    bottom: 16rpx;
    color: white;
    font-size: 20rpx;
  }

  .index-list-text {
    font-size: 24rpx;
    line-height: 66rpx;
    overflow: hidden;
  }

  .twe {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
</style>
