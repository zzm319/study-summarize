<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">首页汇总</text>
    </view>

    <view class="btn-area" @click="handleJumpLogin">
      <view class="btn">跳转登录页面</view>
    </view>
  </view>
</template>

<script>

export default {
  data() {
    return {
      title: 'Hello',
    }
  },
  onLoad() { },
  methods: {
    handleJumpLogin(){
      uni.navigateTo({
        url:'/pages/login/index'
      })
    }
  },
}
</script>

<style lang="less">
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}

.btn-area {
  width: 100%;
  display: flex;
  justify-content: center;
  margin-top: 50rpx;

  .btn {
    width: 480rpx;
    height: 80rpx;
    border: 2rpx solid green;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>
