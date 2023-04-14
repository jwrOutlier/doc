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
