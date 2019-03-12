### iview-admin 配置
### 基础信息
- [iview 官网](https://www.iviewui.com/docs/guide/start)
- [iview github](https://github.com/iview)
- [iview-admin github](https://github.com/iview/iview-admin) / [iview-admin 使用文档](https://lison16.github.io/iview-admin-doc/#/)

### 开始开发
iview-admin建议使用[简化版模板](https://github.com/iview/iview-admin/tree/template)

根据习惯安装 sass-loader && node-sass（使用less就不必安装了）
```javascript
    npm install sass-loader --save-dev 
    npm install node-sass --save-dev 
```
解决npm run dev 报错 [《vue-cli3设置iview自定义主题》](https://my.oschina.net/zjhlearn/blog/1920642?tdsourcetag=s_pcqq_aiomsg) 在`vue.config.js`中添加以下代码
```
 css: {
    loaderOptions: {
      less: {
        javascriptEnabled: true
      }
    }
  }
```

设置代理

```javascript
devServer: {
  	proxy: {
  		'/api': {
  			target: 'https://recsysdev_api.pdnews.cn',
  			changeOrigin: true,
  			pathRewrite: {
  				'^/api': ''
  			}
  		}
  	}
  }
```
在没有home页的情况下，修改home页内容如下：
```html
<Card style="text-align:center">
	<h2>欢迎来到运营管理系统</h2>
	<Divider />
	<div style="padding: 100px 0;">
		<p>今天是：{{today}}</p>
	</div>
</Card>
```
computed属性中增加today
``` javascript
computed: {
	today: function () {
		let dayMap = ['日', '一', '二', '三', '四', '五', '六']
		let nowDate = new Date();
		let _date = nowDate.getFullYear() + '年' + (nowDate.getMonth() + 1) + '月' + nowDate.getDate() + '日，' + '星期' + dayMap[nowDate.getDay()];
		return _date;
	}
}
```

退出时清空页签，避免在一级域名下部署多个项目后造成的混乱。`store/modle/user.js`中引入 `setTagNavListInLocalstorage`，然后在logout中调用

```javascript
import {
	setToken,
	getToken,
	setTagNavListInLocalstorage
} from '@/libs/util'
```

``` javascript

logout(state.token).then(() => {
	setTagNavListInLocalstorage([]);
	commit('setToken', '')
	commit('setAccess', [])
	resolve()
}).catch(err => {
	reject(err)
})

```