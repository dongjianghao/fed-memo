## vue-cli 配置微信分享

此方法仅适用于 vue-cli 脚手架使用。本功能使用了`腾讯移动WEB开发平台`的[对外分享组件](http://open.mobile.qq.com/api/component/share)。传统项目请参考该组件内的示例。

#### 组件功能

定制微信，手机 QQ，QQ 空间 APP 内的分享内容。

#### 组件下载地址

?> http://qzonestyle.gtimg.cn/qzone/qzact/common/share/share.js

#### router/index.js 中添加方法

```js
router.beforeEach((to, from, next) => {
    // 记录第一次进入的URL
    if (!store.state.app.jsUrl) {
        store.dispatch("setShareUrl", document.URL);
    }
    next();
});
```

#### vuex 中添加方法

```js
export default {
    state: {
        jsUrl: "",
        defaultShare: {
            title: "分享标题", // 分享标题
            summary: "分享内容", // 分享内容
            pic: window.location.origin + "/images/logo.png", // 分享图片
            url: window.location.href // 分享链接
        },
        shareData: {}
    },
    mutations: {
        SET_SHARE_URL(state, url) {
            state.jsUrl = url;
        },
        SET_SHARE_DATA(state, shareData) {
            state.defaultShare.url = window.location.href;
            state.shareData = shareData
                ? Object.assign(state.defaultShare, shareData)
                : Object.assign({}, state.defaultShare);
        }
    },
    actions: {
        // 修改分享数据
        setShareData({ commit }, options) {
            commit("SET_SHARE_DATA", options);
            this.dispatch("setTxShare");
        },
        //
        setShareUrl({ commit }, url) {
            commit("SET_SHARE_URL", url);
        },
        // 设置腾讯相关分享（qq、qz、wechat）
        setTxShare({ commit }) {
            let store = this;
            let jsUrl = ""; // 验证签名所需URL

            const ua = window.navigator.userAgent || "";
            const isIOS = /iphone|ipad|ipod/i.test(ua);
            const isWechat = /micromessenger\/([\d.]+)/i.test(ua);

            if (isIOS && isWechat) {
                // 微信浏览器&&苹果设备, 取出记录的第一次访问的URL
                jsUrl = encodeURIComponent(store.state.app.jsUrl.split("#")[0]);
            } else {
                jsUrl = encodeURIComponent(location.href.split("#")[0]); // 当前页面URL
            }
            // 微信中发起请求配置
            if (isWechat) {
                // 根据自己需求替换请求
                getWxConfig({
                    url: jsUrl
                }).then(res => {
                    // 这里调用腾讯对外分享组件
                    /** response
                     * {
                            "error":0,
                            "data":{
                                "appId":"wxd87a5b41c9a3f6b7",
                                "timestamp":1551771917,
                                "nonceStr":"qvumtyviLdVJDi1h",
                                "url":"https://rmrbh5test.peopleapp.com/videoshow/index.html",
                                "signature":"38ca6e02af37b7de316a82f23c0d4fe4ccec4954"
                            }
                        }
                    */
                    setShareInfo({
                        ...store.state.app.shareData,
                        WXconfig: {
                            ...res.data
                        }
                    });
                });
            } else {
                setShareInfo(store.state.app.shareData);
            }
        }
    }
};
```

#### page中调用

```js
this.$store.dispatch("setShareData", {
    title: this.dataInfo.title,
    summary: this.dataInfo.introduction,
    pic: this.dataInfo.cover
});
```
