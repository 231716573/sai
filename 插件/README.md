# 写Blog过程中常用到的插件


#### 1、bootstrap
布局框架，大家都懂的，只能说真的是适合懒人专用


#### 2、jquery
用到烂。。。

#### 3、UEditor
[http://ueditor.baidu.com/website/index.html](http://ueditor.baidu.com/website/index.html)
编辑器，以前学php有试过一个tower开发的Simditor，就用过一次，功能不是很喜欢<br />
UEditor用得多了，需要用到的东西：
```
<!-- 引入JS文件 -->
<script type="text/javascript" charset="utf-8" src="ueditor.config.js"></script>
<script type="text/javascript" charset="utf-8" src="ueditor.all.min.js"> </script>
<!--建议手动加在语言，避免在ie下有时因为加载语言失败导致编辑器加载失败-->
<!--这里加载的语言文件会覆盖你在配置项目里添加的语言类型，比如你在配置项目里配置的是英文，这里加载的中文，那最后就是中文-->
<script type="text/javascript" charset="utf-8" src="lang/zh-cn/zh-cn.js"></script>

<!-- 生成编辑器 -->
<script id="editor" type="text/plain" style="width:1024px;height:500px;"></script>

<!-- 编辑器操作 -->
<div>
  <button onclick="getAllHtml()">获得整个html的内容</button>
  <button onclick="getContent()">获得内容</button>
  <button onclick="setContent()">写入内容</button>
  <button onclick="setContent(true)">追加内容</button>
  <button onclick="getContentTxt()">获得纯文本</button>
  <button onclick="getPlainTxt()">获得带格式的纯文本</button>
  <button onclick="hasContent()">判断是否有内容</button>
  <button onclick="setFocus()">使编辑器获得焦点</button>
  <button onmousedown="isFocus(event)">编辑器是否获得焦点</button>
  <button onmousedown="setblur(event)" >编辑器失去焦点</button>

</div>
<div>
  <button onclick="getText()">获得当前选中的文本</button>
  <button onclick="insertHtml()">插入给定的内容</button>
  <button id="enable" onclick="setEnabled()">可以编辑</button>
  <button onclick="setDisabled()">不可编辑</button>
  <button onclick=" UE.getEditor('editor').setHide()">隐藏编辑器</button>
  <button onclick=" UE.getEditor('editor').setShow()">显示编辑器</button>
  <button onclick=" UE.getEditor('editor').setHeight(300)">设置高度为300默认关闭了自动长高</button>
</div>

<div>
  <button onclick="getLocalData()" >获取草稿箱内容</button>
  <button onclick="clearLocalData()" >清空草稿箱</button>
</div>
<button onclick="createEditor()">
创建编辑器</button>
<button onclick="deleteEditor()">
删除编辑器</button>


<!-- JS操作代码 -->
//实例化编辑器
//建议使用工厂方法getEditor创建和引用编辑器实例，如果在某个闭包下引用该编辑器，直接调用UE.getEditor('editor')就能拿到相关的实例
var ue = UE.getEditor('editor');

function isFocus(e){
    alert(UE.getEditor('editor').isFocus());
    UE.dom.domUtils.preventDefault(e)
}
function setblur(e){
    UE.getEditor('editor').blur();
    UE.dom.domUtils.preventDefault(e)
}
function insertHtml() {
    var value = prompt('插入html代码', '');
    UE.getEditor('editor').execCommand('insertHtml', value)
}
function createEditor() {
    enableBtn();
    UE.getEditor('editor');
}
function getAllHtml() {
    alert(UE.getEditor('editor').getAllHtml())
}
function getContent() {
    var arr = [];
    arr.push("使用editor.getContent()方法可以获得编辑器的内容");
    arr.push("内容为：");
    arr.push(UE.getEditor('editor').getContent());
    alert(arr.join("\n"));
}
function getPlainTxt() {
    var arr = [];
    arr.push("使用editor.getPlainTxt()方法可以获得编辑器的带格式的纯文本内容");
    arr.push("内容为：");
    arr.push(UE.getEditor('editor').getPlainTxt());
    alert(arr.join('\n'))
}
function setContent(isAppendTo) {
    var arr = [];
    arr.push("使用editor.setContent('欢迎使用ueditor')方法可以设置编辑器的内容");
    UE.getEditor('editor').setContent('欢迎使用ueditor', isAppendTo);
    alert(arr.join("\n"));
}
function setDisabled() {
    UE.getEditor('editor').setDisabled('fullscreen');
    disableBtn("enable");
}

function setEnabled() {
    UE.getEditor('editor').setEnabled();
    enableBtn();
}

function getText() {
    //当你点击按钮时编辑区域已经失去了焦点，如果直接用getText将不会得到内容，所以要在选回来，然后取得内容
    var range = UE.getEditor('editor').selection.getRange();
    range.select();
    var txt = UE.getEditor('editor').selection.getText();
    alert(txt)
}

function getContentTxt() {
    var arr = [];
    arr.push("使用editor.getContentTxt()方法可以获得编辑器的纯文本内容");
    arr.push("编辑器的纯文本内容为：");
    arr.push(UE.getEditor('editor').getContentTxt());
    alert(arr.join("\n"));
}
function hasContent() {
    var arr = [];
    arr.push("使用editor.hasContents()方法判断编辑器里是否有内容");
    arr.push("判断结果为：");
    arr.push(UE.getEditor('editor').hasContents());
    alert(arr.join("\n"));
}
function setFocus() {
    UE.getEditor('editor').focus();
}
function deleteEditor() {
    disableBtn();
    UE.getEditor('editor').destroy();
}
function disableBtn(str) {
    var div = document.getElementById('btns');
    var btns = UE.dom.domUtils.getElementsByTagName(div, "button");
    for (var i = 0, btn; btn = btns[i++];) {
        if (btn.id == str) {
            UE.dom.domUtils.removeAttributes(btn, ["disabled"]);
        } else {
            btn.setAttribute("disabled", "true");
        }
    }
}
function enableBtn() {
    var div = document.getElementById('btns');
    var btns = UE.dom.domUtils.getElementsByTagName(div, "button");
    for (var i = 0, btn; btn = btns[i++];) {
        UE.dom.domUtils.removeAttributes(btn, ["disabled"]);
    }
}

function getLocalData () {
    alert(UE.getEditor('editor').execCommand( "getlocaldata" ));
}

function clearLocalData () {
    UE.getEditor('editor').execCommand( "clearlocaldata" );
    alert("已清空草稿箱")
}
```

#### 4、uploadify
[http://www.uploadify.com/](http://www.uploadify.com/)
上传图片插件，需要用到得代码<br />
需要用到的东西：
```
<!-- 需要引入的文件 -->
<script src="jquery.uploadify.min.js" type="text/javascript"></script>
<link rel="stylesheet" type="text/css" href="uploadify.css">

<!-- 生成上传按钮 -->
<input id="file_upload" name="file_upload" type="file" multiple="true">


<!-- JS操控代码 -->
<?php $timestamp = time();?>
$(function() {
    $('#file_upload').uploadify({
        'buttonText' : '上传缩略图',  // 修改按钮文字
        'formData'     : {
            'timestamp' : '<?php echo $timestamp;?>',
            'token'     : '<?php echo md5('unique_salt' . $timestamp);?>'
        },
        'swf'      : 'uploadify.swf',
        'uploader' : 'uploadify.php'
    });
});
```

#### layui 
[http://www.layui.com/](http://www.layui.com/)

弹窗工具，只要是用来代替系统一些特定的弹窗，官网有使用教程，比如：
```
alert、confirm......这些 
```

同时发现上面有一些写好的效果，比如弹窗预览图片啊。但是确定是太大了，有200多K
