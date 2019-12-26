---
title: 小程序swiper组件ios低版本样式兼容性
date: 2019-12-26 17:34:11
tags: 小程序
categories: 小程序
---

### 问题

最近在开发企业微信内部使用的小程序,收到同事的反馈,在系统版本为ios 10.3.3的手机下,页面内容显示不全,当时的定位就是页面中使用了 `flex` 布局,在低版本的手机下可能存在内容不能撑满.但是在进一步调试中,发现 `swiper` 组件的 `height: 100%` 高度并未起效.

### 分析与解决

当前布局为上中下类型,下图中的A和C位置固定,但是A的高度不定,C高度固定,B部分的内容即剩余高度,当内容高度大于屏幕剩余高度时,可以滚动.

![](/images/2019-12-26-01.png)

本来这种布局用 `flex` 布局是没问题的,但是,问题的关键是,B还可以 `tab` 切换,需要引入 `swiper` 组件,此时布局就变成了如下:

``` html
<view class="detail">
  <view class="header">A</view>
  <view class="tab">
    <view class="item">B-1</view>
    <view class="item">B-2</view>
  </view>
  <view class="content" id="contentId">
    <swiper class="swiper" style="{{swiperHeight ? 'height: '+ swiperHeight + 'px;': ''}}" circular="true" current="{{currentPage}}" bindchange="tabChange">
      <swiper-item class="swiperItem">
        <scroll-view scroll-y="true" class="scroll" enable-back-to-top="{{true}}">
        ...
        </scroll-view>
      </swiper-item>
      <swiper-item class="swiperItem">
        <scroll-view scroll-y="true" class="scroll" enable-back-to-top="{{true}}">
        ...
        </scroll-view>
      </swiper-item>
    </swiper>
  </view>
  <view class="footer">C</view>
</view>
```

``` css
.detail{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 100%;
}
.header{
  width: 100%;
}
.tab{
  width: 100%;
  height: 80rpx;
}
.content{
  flex: 1;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.swiper, .scroll{
  width: 100%;
  height: 100%;
}
.footer{
  width: 100%;
  height: 112rpx;
}
```

由于A的高度不定,需要根据接口请求返回的数据来计算B部分的高度,具体代码如下:

``` js
Page({
  data: {
    swiperHeight: 0
  },
  onLoad() {
    // 数据请求后进行高度计算
    setTimeout(() => {
      queryNodeInfoPromise('#contentId').then((rect) => {
        console.error('rect: ', rect)
        if (rect) {
          const system = wx.getSystemInfoSync();
          const pixelRatio = system.pixelRatio || 2;
          const btnHeight = Math.floor((BTNS_HEIGHT / pixelRatio) * 100) / 100;
          this.swiperHeight1 = rect.height;
          this.swiperHeight2 = rect.height + btnHeight;
          this.setData({
            swiperHeight: this.swiperHeight1
          });
        }
      });
    }, 0);
  }
});

function queryNodeInfoPromise(nodeId: string): Promise<any> {
  if (typeof wx.createSelectorQuery !== 'function') {
    return Promise.resolve(null);
  }
  const query = wx.createSelectorQuery()
  return new Promise((resolve) => {
    query.select(nodeId).boundingClientRect((rect) => {
      resolve(rect)
    }).exec()
  })
}
```

### 小结

1. 对于特殊的布局, `swiper` 组件 `height: 100%` 的样式未生效,只能通过根据内容来计算组件的高度,设置具体的高度值.

2. 在查找问题时,由于企业微信不能使用调试工具进行真机调试,错失了快速定位问题的原因.反思,定位问题可以借助微信进行真机调试,快速定位布局原因.