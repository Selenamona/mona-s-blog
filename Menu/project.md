---
layout: post
title: Project
---

 
## | 遇到的问题

**页面空白原因**

- ES6 兼容问题，安装插件 `babel-polyfill": "^6.26.0`
- vue-cli config 中文件不转译 ES6
- 外部引入 vux，样式影响
- 代码报错 

**token 获取**

背景：H5 嵌在原生，首页打包放原生本地，首页跳转页面调原生方法。

问题：首页获取到的token本地存储，其他页面获取不到。

解决办法：原生打开 H5 页面，路径后拼接参数，H5 本地存储。


## | 功能实现

**复制**

安装 "clipboard": "^2.0.4"

```html
<span ref="copy_el"><span id="adminUrl">1111</span></span>
<button 
    class="btnCopy" 
    data-clipboard-action="copy" 
    data-clipboard-target="#adminUrl"
>复制</button>
```
```javascript
import Clipboard from "clipboard"   
let clipboard = new Clipboard('.btnCopy');
clipboard.on('success', (e) => {  // 复制成功
    e.clearSelection();  // 清除选中样式（蓝色）
});
clipboard.on('error', (e) => { // 复制失败
    // todo...
});
```

**输入验证码**

```html
<!-- 忽略反斜杆 -->
<div class="code">
    <ul class="num">
        <li>{\{code.charAt(0)\}}</li>
        <li>{\{code.charAt(1)\}}</li>
        <li>{\{code.charAt(2)\}}</li>
        <li>{\{code.charAt(3)\}}</li>
        <li>{\{code.charAt(4)\}}</li>
        <li>{\{code.charAt(5)\}}</li>
        <input type="tel" maxlength="6" class="ipt" v-model="code" @input="codeIpt">
    </ul>
    <button @click="getCode" v-show="!ifSend">获取验证码</button>
    <button class="reSendMsg" v-show="ifSend">重新获取({{second}})</button>
</div>
```

```javascript
// 输入验证码
codeIpt(){
    this.code = this.code.replace(/[^\d]/, ''); // 只输入数字
},
// 获取验证码
getCode(){
    let timer = setInterval(() => {
        this.second = this.second - 1;
        if(this.second === 0) { 
            this.second = 59;
            clearInterval(timer);
        }
    }, 1000)

    // 调用获取验证码的接口 
}
```

```less
.code {
    margin: .36rem 0 .13rem;
    display: flex;
    justify-content: space-between;
    .num {
        display: flex;
        justify-content: space-around;
        position: relative;
        li {
            border: 1px solid #EEEEEE;
            border-radius: 2px;
            width: .34rem;
            height: .34rem;
            line-height: .34rem;
            margin-right: .03rem;
            text-align: center;
            font-family: PingFangSC-Medium;
            font-size: .18rem;
            color: #333333;
        }
        .ipt {
            position: absolute;
            width: 200%;
            height: 100%;
            background: transparent;
            left: -100%; 
            top: 0;
            text-indent: -999em;
        }
    } 
}
```


**输入框问题汇总**

- 输入框失焦事件与按钮点击事件冲突

    iPhone onclick事件有大约半秒的延迟，这是因为 iOS 系统需要等待一段时间来判断用户是点击还是拖动，所以会有失焦与点击事件的冲突。
    
    问题描述：
    给一个输入框设置失焦事件之后，失焦事件总是优先其它事件先触发，导致输入完成之后点击提交按钮无效。
    
    拓展： ①mousedown：在用户按下了任意鼠标按钮时触发。不能通过键盘触发这个事件。
           ②mouseup：在用户释放鼠标按钮时触发。不能通过键盘触发这个事件。
    
    解决方案：
    方法1、通过给失焦事件设置延迟触发。
    方法2、将按钮的点击（click）事件改为按下（mousedown）事件


- 输入框失焦事件与按钮点击事件冲突

    问题描述：H5页面在 IOS 键盘收起时，页面不回弹，导致底部大面积空白；
    问题复现：底部输入框聚焦显示键盘，IOS 点击"完成"，输入框失焦键盘收起，底部空白
    解决方案：input 失焦事件添加：`window.scroll(0,0)`

    ```js
    $("input").on("blur",function(){
        window.scroll(0,0);//失焦后强制让页面归位
    });
    ```


日期格式兼容 new Date()




 