# 实用工具

1. 前端文件下载
   - 对文件地址下载
    ```js
        function downloadFile (url, fileName = '') {
            let link = document.createElement('a')
            link.style.display = 'none'
            link.href = url
            link.download = fileName
            document.body.appendChild(link)
            link.click()
            document.body.removeChild(link)
            window.URL.revokeObjectURL(url) // 释放
        }
    ```
    - 对文件流下载
    ```js
        // '.csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel'  下载cve类型
        function downloadBlob2 (res, filename,type) {
            // 创建blob对象，解析流数据
            const blob = new Blob([res], {type:type })
            // 这里就是创建一个a标签，等下用来模拟点击事件
            const a = document.createElement('a')
            // 兼容webkix浏览器，处理webkit浏览器中href自动添加blob前缀，默认在浏览器打开而不是下载
            const URL = window.URL || window.webkitURL
            // 根据解析后的blob对象创建URL 对象
            const herf = URL.createObjectURL(blob)
            // 下载链接
            a.href = herf
            // 下载文件名,如果后端没有返回，可以自己写a.download = '文件.pdf'
            a.download = filename
            document.body.appendChild(a)
            // 点击a标签，进行下载
            a.click()
            // 收尾工作，在内存中移除URL 对象
            document.body.removeChild(a)
            window.URL.revokeObjectURL(herf)
        }
    ```

2. 大屏缩放组件
```vue
<template>
  <div class="ScreenAdapter" :style="style">
    <slot />
  </div>
</template>
<script>
export default {
  name: 'ScreenAdapter',
  props: {
    width: {
      type: Number,
      default: 1920 // 设计稿的大小  记住不能动态的改变这个的大小
    },
    height: {
      type: Number,
      default: 937 // 设计稿的大小 记住不能动态的改变这个的大小
    }
  },
  data () {
    return {
      style: {
        width: this.width + 'px',
        height: this.height + 'px',
        transform: 'scale(1) translate(-50%, -50%)'
      }
    }
  },
  mounted () {
    this.setScale()
    window.onresize = this.Debounce(this.setScale, 400)
  },
  methods: {
    Debounce: (fn, t) => {
      const delay = t || 500
      let timer
      return function () {
        const args = arguments
        if (timer) {
          clearTimeout(timer)
        }
        const context = this
        timer = setTimeout(() => {
          timer = null
          fn.apply(context, args)
        }, delay)
      }
    },
    // 获取放大缩小比例
    getScale () {
      const width = window.innerWidth
      const height = window.innerHeight
      const w = width / this.width
      const h = height / this.height
      return w < h ? w : h
      // return w
    },
    // 设置比例
    setScale () {
      this.style.transform = 'scale(' + this.getScale() + ') translate(-50%, -50%)'
    }
  },
  destroyed () {
    window.onresize = null
  }
}
</script>
<style lang="scss" scoped>
.ScreenAdapter {
    transform-origin: 0 0;
    position: absolute;
    left: 50%;
    top: 50%;
    transition: 0.3s;
    padding: 20px;
    box-sizing: border-box;
}
</style>
```

3.vw和vh响应式方案

* sass 方案

```scss
@use "sass:math";

// 默认设计稿的宽度
$designWidth: 1920;
// 默认设计稿的高度
$designHeight: 1080;

// px 转为 vw 的函数  可以设置宽和字体大小
@function vw($px) {
    @return math.div($px, $designWidth) * 100vw;
}

// px 转为 vh 的函数
@function vh($px) {
    @return math.div($px, $designHeight) * 100vh;
}


.App {
    width: vw(400);
    height: vh(300);
    background-color: aqua;
    font-size: vw(16);
}
```

* less 方案

```less
@charset "utf-8";

// 默认设计稿的宽度
@designWidth: 1920;

// 默认设计稿的高度
@designHeight: 1080;

.px2vw(@name, @px) {
  @{name}: (@px / @designWidth) * 100vw;
}

.px2vh(@name, @px) {
  @{name}: (@px / @designHeight) * 100vh;
}

.px2font(@px) {
  font-size: (@px / @designWidth) * 100vw;
}

```

4. 大屏适配方案scale 解决留白问题

```js
const autofit = {
  init(options) {
    let designWidth = options?.designWidth || 1920;
    let designHeight = options?.designHeight || 929;
    let renderDom = options?.renderDom || "#app";
    let resize = options?.resize || true;
    document.querySelector(renderDom).style.height = `${designHeight}px`;
    document.querySelector(renderDom).style.width = `${designWidth}px`;
    document.querySelector(renderDom).style.transformOrigin = `0 0`;
    keepFit(designWidth, designHeight, renderDom);
    resize && (window.onresize = () => {
      keepFit(designWidth, designHeight, renderDom);
    })
  }
}

function keepFit(designWidth, designHeight, renderDom) {
  let clientHeight = document.documentElement.clientHeight;
  let clientWidth = document.documentElement.clientWidth;
  let scale = 1;
  if (clientWidth / clientHeight < designWidth / designHeight) {
    scale = (clientWidth / designWidth)
    document.querySelector(renderDom).style.height = `${clientHeight / scale}px`;
  } else {
    scale = (clientHeight / designHeight)
    document.querySelector(renderDom).style.width = `${clientWidth / scale}px`;
  }
  document.querySelector(renderDom).style.transform = `scale(${scale})`;
}
```

5. 轮播swiper的用法

> swiper版本`^9.1.0`  注意：每个版本可能不同

用法：
```html
<div class="swiper-container swiper">
    <div class="swiper-wrapper">
        <div class="swiper-slide">
            <div class="inner-item"></div>
        </div>
    </div>
</div>
```
> 注意：swiper-container  要设置固定高度

```js
import Swiper,{ Autoplay } from 'swiper';
import 'swiper/swiper-bundle.css';
Swiper.use([Autoplay])

new Swiper('.swiper-container', {
    direction: 'vertical',  // 方向
    spaceBetween: 26,
     // 改变swiper样式时，自动初始化swiper
    observer: true,
    // 监测swiper父元素，如果有变化则初始化swiper
    observeParents: true,
    loop: true, // 是否循环滑动
    slidesPerView: 5, // 每次滑动显示几个Slide
    autoplay: {
      delay: 2000, // 自动切换的时间间隔
      disableOnInteraction: false, // 用户操作后是否停止自动切换
    },
})
```

5. 打印页面

设置打印页面的样式
```css
    @media print {
    }
```

6. git 常用命令

```
// 合并分支上某一个提交
git cherry-pick 430b5faa84a813eb06e770925b09c82553b2e10
```
