# WXGameBanner

小游戏推荐banner组件。小游戏推荐banner组件是一个原生组件，层级比上屏 Canvas 高，会覆盖在上屏 Canvas 上。小游戏推荐banner组件默认是隐藏的，需要调用 GameBanner.show() 将其显示。

```typescript
//
WXGameBanner.show("PBgAAdIb599ZiJlI");
//
WXGameBanner.hide() ;
```

> 辅助类

```typescript
module WXGameBanner {

    var _gameBannerAD = null;

    export function show(adUid: string, obj: Object = null): void {
        // 基础库版本号 >= 2.7.5 后再使用该 API
        if (WXVerson.compareSDK("2.7.5")) {
            if (_gameBannerAD) {
                _gameBannerAD.offResize(onGameBannerResize);
                _gameBannerAD.destroy();
            }

            if (wx.createGameBanner) {
                let screenWidth = WXDevice.getInfo(SystemInfoKey.screenWidth) ;
                let screenHeight = WXDevice.getInfo(SystemInfoKey.screenHeight) ;
                let styleObj = obj ? obj : { left: 0, top: screenHeight, width: screenWidth };
                //
                _gameBannerAD = wx.createGameBanner({
                    adUnitId: adUid, style: styleObj
                });
                _gameBannerAD.onResize(onGameBannerResize);
                _gameBannerAD.show();
            }
        }
    }

    function onGameBannerResize(res) {
        if (_gameBannerAD) {
            let screenHeight = WXDevice.getInfo(SystemInfoKey.screenHeight);
            _gameBannerAD.style.top = screenHeight - res["height"];
        }
    }

    /** 隐藏Banner广告 */
    export function hide(): void {
        if (_gameBannerAD) {
            _gameBannerAD.hide();
        }
    }

}
window['WXGameBanner'] = WXGameBanner;
```

