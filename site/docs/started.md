---
title: 快速上手
---

#### 脚手架

**新项目**可通过[vue-cli](https://github.com/vuejs/vue-cli)初始化集成`mand-mobile`，**现有项目**集成请参考[安装](#docs/started?anchor=安装)

* vue cli 2.0([模板](https://github.com/mand-mobile/mand-mobile-template))

```shell
npm install -g vue-cli
vue init mand-mobile/mand-mobile-template my-mand-mobile-project
```

* vue cli 3.0(参考[示例](https://github.com/mand-mobile/mand-mobile-vue-cli3-example))

```shell
npm install -g @vue/cli
vue create my-mand-mobile-project
```

#### 安装

##### NPM

```shell
npm install mand-mobile --save
```

##### CDN

```html
<link rel="stylesheet" href="https://unpkg.com/mand-mobile/lib/mand-mobile.css">
<script src="https://unpkg.com/mand-mobile/lib/mand-mobile.umd.js"></script>
```

#### 引入

##### 按需加载(推荐)

> 使用 <a href="https://github.com/ant-design/babel-plugin-import" target="_blank">babel-plugin-import</a>
  或
  <a href="https://github.com/Brooooooklyn/ts-import-plugin" target="_blank">ts-import-plugin</a>

```javascript
// .babelrc
{
  plugins: [
    [import, {
      libraryName: "mand-mobile"
    }]
  ]
}
```

```javascript
// ts-loader option
{
  rules: [
  	{
	   test: /\.tsx?$/,
	   loader: 'ts-loader',
	   options: {
	     appendTsSuffixTo: [/\.vue$/],
	     transpileOnly: true,
	     getCustomTransformers: () => ({
	       before: [
            require('ts-import-plugin')({
              "libraryName": "mand-mobile"
            })
	       ]
	     })
	   }
    }
  ]
}
```

组件使用：

```javascript
import { Button } from 'mand-mobile'
```

##### 按需引入

```javascript
import Button from 'mand-mobile/lib/button'
```

##### 全量引入

```javascript
import Vue from 'vue'
import mandMobile from 'mand-mobile'

Vue.use(mandMobile)
```

#### 使用前准备

> 为避免因[浏览器兼容性](https://developer.mozilla.org/en-US/docs/Web/Events/click#Safari_Mobile)引起的点击问题，建议引入[FastClick](https://github.com/ftlabs/fastclick)

##### `px` to `rem`

组件样式以`px`为单位，并且以`iPhone6`屏幕 “物理像素” 宽度`750`为基准 (即普通 “逻辑像素” 值的`2`倍大小)。在实际项目中，可根据具体情况使用`postcss-pxtorem`把`px`单位转成`rem`，从而实现不同设备下等比缩放的效果。

如转换基准为`1rem = 100px`：

* `.postcssrc.js`配置

```javascript
module.exports = {
  'plugins': [
    require('postcss-pxtorem')({
      rootValue: 100,
      propWhiteList: []
    })
  ]
}
```

* `webpack`配置

```javascript
const pxtorem = require('postcss-pxtorem');

// Postcss
webpackConfig.postcss.push(pxtorem({
  rootValue: 100,
  propWhiteList: []
}))

// Poststylus（使用源码时）
const poststylus = require('poststylus')

webpackConfig.plugins.push(new webpack.LoaderOptionsPlugin({
  options: {
    stylus: {
      use: [
        poststylus(pxtorem({
          rootValue: 100,
          propWhiteList: []
        }))
      ]
    }
  }
}))
```

#### 使用

这是一个使用`Mand Mobile`组件开发而成的表单页面，更多的组件使用方法可在<a href="#/docs/preview">组件概览</a>中找到。

```vue
<template>
  <div id="app">
    <md-field title="投保人" class="block">
      <md-input-item
        title="投保人姓名"
        placeholder="请填写投保人姓名"
      ></md-input-item>
      <md-input-item
        title="身份证号"
        placeholder="请填写投保人身份证号"
      ></md-input-item>
    </md-field>
    <md-field title="被保人" class="block">
      <md-input-item
        title="被保人姓名"
        placeholder="请填写被保人姓名"
      ></md-input-item>
      <md-field-item
        title="与投保人关系"
        arrow="arrow-right"
        :value="relation"
        @click="isPickerShow = true"
        solid
      ></md-field-item>
      <md-picker
        v-model="isPickerShow"
        :data="pickerData"
        title="选择与投保人关系"
      ></md-picker>
      <md-input-item
        title="身份证号"
        placeholder="请填写被保人身份证号"
      ></md-input-item>
      <md-input-item
        title="手机号"
        type="phone"
        placeholder="请填写被保人手机号"
      ></md-input-item>
    </md-field>
    <md-agree class="agree">
      本人承诺投保人已充分了解本保险产品，并保证投保信息的真实性，理解并同意
    </md-agree>
    <md-action-bar :actions="actionBarData">
      &yen;128.00
    </md-action-bar>
  </div>
</template>

<script>
import {
  Agree,
  ActionBar,
  Field,
  FieldItem,
  InputItem,
  Picker
} from 'mand-mobile'

export default {
  name: 'app',

  components: {
    [Agree.name]: Agree,
    [ActionBar.name]: ActionBar,
    [Field.name]: Field,
    [FieldItem.name]: FieldItem,
    [InputItem.name]: InputItem,
    [Picker.name]: Picker
  },

  data () {
    return {
      relation: '本人',
      isPickerShow: false,
      actionBarData: [{
        text: '我要投保'
      }],
      pickerData: [[{text:'本人'},{text:'父母'},{text:'配偶'},{text:'子女'}]]
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
body{
  background: #f0f0f0;
}
.detail{
  background: #fff;
  font-size: .24rem;
}
.block{
  margin-top: .32rem;
}
.agree{
  padding: .32rem;
  font-size: .24rem;
  color: #666;
}
</style>
```

