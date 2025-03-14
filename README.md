# 下载安装

下载 [PrintEase-1.0.0.exe](https://github.com/herohql/printEase/raw/refs/heads/main/PrintEase-1.0.0.exe)，启动后服务地址http://localhost:8000，如果端口冲突会自动递增



# DEMO

浏览器打开 my-print.html ，web打印案例已经写好， 详细api文档见下方

# web获取打印机列表

get请求，地址：http://localhost:8000/getPrintList

```js
let printerName = "";
fetch("http://localhost:8000/getPrintList", {
	method: "GET",
headers: { "Content-Type": "application/json" },
})
.then((response) => response.json())
.then((data) => {
  console.log("Response:", data.data);
  printerName = data.data.filter((e) => e.isDefault === true)[0].name;
});
```

# 打印(arrayBuffer )

支持文档流打印

post请求，地址：http://localhost:8000/print

url参数：

| Query 参数  | 类型           | 解释                                     |
| ----------- | -------------- | ---------------------------------------- |
| printerName | string         | 打印机名称                               |
| duplex      | boolean/string | false(boolean/string) 单面  ；'long'双面 |
| paperSize   | string         | 纸张大小 'A4'                            |

body参数：arrayBuffer  （Blob 转换为 ArrayBuffer方法见下方）

## fetch方法

```js
fetch(`http://localhost:8000/print?printerName=${printerName}`, {
	method: "POST",
headers: {
  "Content-Type": "application/pdf",
},
body: arrayBuffer,
})
.then((response) => response.text())
.then((data) => {
 	alert(data);
})
.catch((error) => {
	console.error("发送失败:", error);
	alert("发送失败");
});
```

## axios方法

```js
const queryString = new URLSearchParams({ printerName: '', duplex:false, paperSize:'A4'}).toString()
axios
.post(`http://localhost:8000/print?${queryString}`, arrayBuffer, {
  headers: {
    'Content-Type': 'application/pdf'
  }
})
.then((response) => {
  
})
.catch((error) => {
  console.error('请求失败:', error)
})
```



## Blob 转 ArrayBuffe

如果后端返的是Blob，这里转换一下

```js
// 创建一个 FileReader 对象来将 Blob 转换为 ArrayBuffer
  const reader = new FileReader()
  reader.onloadend = function () {
    pdfArrayBuffer = reader.result
  }
  reader.readAsArrayBuffer(Blob)
```

# 打印(url)

支持网络地址的pdf打印，参数一样

## fetch方法

```js
const url = 'https://pdf-lib.js.org/assets/with_update_sections.pdf'
fetch(url).then(response=>{
  return response.arrayBuffer();
}).then(arrayBuffer=>{
  console.log("PDF ArrayBuffer:", arrayBuffer);
})
```

## axios方法

```js
axios.get(url, { responseType: 'arraybuffer' }).then((response) => {
  const arrayBuffer = response.data
})
```

# 使用限制

【免费版】 --  提供100次打印

【付费版1】-- 不限制打印次数

【付费版2】 --不限制打印次数，提供源码

联系作者主页查看微信

