# postcss-px-2-vp-pro
优化postcss-px-to-viewport，vite项目中支持postcss8，viewportWidth配置项支持函数，根据条件返回设计稿尺寸

安装：
```bash
yarn add postcss-px-2-vp-pro -D
```

postcss配置如下：

```js
// 以postcss.config.js 为例

const path = require('path');
module.exports = () => {
  return {
    plugins: {
      tailwindcss: {},// 用来给不同的浏览器自动添加相应前缀，如-webkit-，-moz-等等
      autoprefixer: {},
      'postcss-px-2-vp-pro': {
        unitToConvert: "px", // 要转化的单位
        // viewportWidth: 750, // UI设计稿的宽度
        viewportWidth: (file) => {
          const reg = /[\\/]node_modules[\\/]vant/g;
          return reg.test(file) ? 375 : 750
        },
        unitPrecision: 3, // 转换后的精度，即小数点位数
        propList: ["*"], // 指定转换的css属性的单位，*代表全部css属性的单位都进行转换
        viewportUnit: "vw", // 指定需要转换成的视窗单位，默认vw
        fontViewportUnit: "vw", // 指定字体需要转换成的视窗单位，默认vw
        selectorBlackList: ["wrap"], // 指定不转换为视窗单位的类名，
        minPixelValue: 1, // 默认值1，小于或等于1px则不进行转换
        mediaQuery: true, // 是否在媒体查询的css代码中也进行转换，默认false
        replace: true, // 是否转换后直接更换属性值
        // exclude: [/node_modules/], // 设置忽略文件，用正则做目录名匹配
        landscape: false, // 是否处理横屏情况
      }
    }
  }
};
```