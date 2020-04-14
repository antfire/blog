# 微信小游戏 Banner 广告


> **SDKVersion 基础库版本号 >= 2.0.4**

```typescript
// 显示
WXBannerAd.show("adunit-784dd658c0090f31");
// 隐藏
WXBannerAd.hide() ;
```

[compareVerson 函数]: ./微信基础库版本比较.md

<img src="../images/WechatIMG63.png" alt="WechatIMG63" style="zoom: 33%;" />

> **BannerAd **

```typescript
// banner 广告组件是一个原生组件，层级比普通组件高  

interface BannerAd {
        /**
         * banner 广告组件的样式。
         * style 上的属性的值仅为开发者设置的值，
         * banner 广告会根据开发者设置的宽度进行等比缩放，
         * 缩放后的真实尺寸需要通过 BannerAd.onResize() 事件获得。
         */
        style: {
            /** banner 广告组件的左上角横坐标*/
            left: number,
            /** banner 广告组件的左上角纵坐标*/
            top: number,
            /** banner 广告组件的宽度。
            最小 300，最大至 屏幕宽度（屏幕宽度可以通过 wx.getSystemInfoSync() 获取）。*/
            width: number,
            /** banner 广告组件的高度*/
            height: number,
            /** banner 广告组件经过缩放后真实的宽度*/
            realWidth: number,
            /** banner 广告组件经过缩放后真实的高度*/
            realHeight: number
        };

        /** 显示 banner 广告。*/
        show(): Promise<any>;
        /** 隐藏 banner 广告*/
        hide(): void;
        
        /** 销毁 banner 广告*/
        destroy(): void;
        /** 监听 banner 广告尺寸变化事件*/
        onResize(callback: (res: { width: number, height: number }) => void): void;
        /** 取消监听 banner 广告尺寸变化事件*/
        offResize(callback: () => void): void;
        /** 监听 banner 广告加载事件*/
        onLoad(callback: () => void): void;
        /** 取消监听 banner 广告加载事件*/
        offLoad(callback: () => void): void;
        /** 监听 banner 广告错误事件*/
        onError(callback: (res: { errMsg: string, errCode: 1000 | 1001 | 1002 | 1003 | 1004 | 1005 | 1006 | 1007 | 1008 }) => void): void;
        /** 取消监听 banner 广告错误事件*/
        offError(callback: () => void): void;
}


// 创建BannderAd对象
function createBannerAd(res: {
        adUnitId: string,   // 广告单元 id
        adIntervals:number, // 广告自动刷新的间隔时间，单位为秒，参数值必须大于等于30
        style: {
            left?: number,  // banner 广告组件的左上角横坐标
            top?: number,   // banner 广告组件的左上角纵坐标
            width?: number, // banner 广告组件的宽度
            height?: number // banner 广告组件的高度
        }
    }): BannerAd;
```



> 辅助类

```typescript
module WXBannerAd {
  
  var _systemInfo: any;
  var _bannerAD = null;

  /** 显示Banner广告 */
  export function show(adUid: string, obj?: Object): void {
      if(_systemInfo == null){
         _systemInfo = wx.getSystemInfoSync();
      }
      
      if (compareVersion('2.0.4') ) {
          if (_bannerAD) {
              _bannerAD.offResize(onBannerResize);
              _bannerAD.destroy();
          }

          if (wx.createBannerAd) {
              //
              let screenWidth = _systemInfo.screenWidth;
              let screenHeight = _systemInfo.screenHeight;
              //
              let styleObj = obj ? obj : { left: 0, top: screenHeight, width: screenWidth };
                  //
              _bannerAD = wx.createBannerAd({
                  adUnitId: adUid, style: styleObj
              });
              _bannerAD.onResize(onBannerResize);
              _bannerAD.onError(onBannerErr);
              _bannerAD.show();
          }
      }
      else{
          wx.showModal({
                title: '提示',
                content: '当前微信版本过低，无法使用Banner广告功能，请升级到最新微信版本后重试。'
              })
      }
  }

  /** 隐藏Banner广告 */
  export function hide(): void {
       if (_bannerAD) {
           _bannerAD.hide();
       }
  }

  function onBannerResize(res) {
       if (_bannerAD) {
           // 屏幕高度，单位px
           let screenHeight = _systemInfo.screenHeight;
           // 设置banner的top值 : 屏幕高度 - banner组件高度
           _bannerAD.style.top = screenHeight - res["height"];
       }
  }
    
  function onBannerErr(esm) {
       console.error("ON_BANNER_ERR:", esm);
  }
}

```



---

内容如有错误联系方式:  fraser@11h5.com

更新时间：2020-4-9           