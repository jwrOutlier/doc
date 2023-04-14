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
