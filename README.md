# uploadImage-IE-8-

<h2>H5 FileReader选本地图片转base64（兼容IE8）</h2>

<h3>使用例子：</h3>

<p>图片容器 显示选中的图片
<div id="container"><br>
	<img src="" /><br>
</div><br>
 
 
 <p>按钮点击选图
 <div id="uploadImgBtn" class="layerBtn" style="width: 100%;margin-left:0;margin-top: 3px; ">click</div>
 <input onchange="imgChange(this)" type="file" id="getImgfile" accept="image/jpg,image/jpeg,image/gif,image/png" style="display:none"/><br>
//不使用image/* 避免Chrome 下打开窗口缓慢<br>		


<h3>JS</h3>
var imgChange = function(file) {<br>
	if (file.files && file.files[0]) {<br>
		var reader = new FileReader();<br>
		reader.onload = function(evt) {<br>
			$("#previewImg").prop("src",evt.target.result);<br>
		};<br>
		reader.readAsDataURL(file.files[0]);<br>
	} else {//IE 8 can't use FileReader<br>
		var realPath, xmlHttp, xml_dom, tmpNode, imgBase64Data;<br>
		realPath = file.value;//get the image path<br>
		xmlHttp = new ActiveXObject("MSXML2.XMLHTTP");<br>
		xmlHttp.open("POST",realPath, false);<br>
		xmlHttp.send("");<br>
		xml_dom = new ActiveXObject("MSXML2.DOMDocument");<br>
		tmpNode = xml_dom.createElement("tmpNode");<br>
		tmpNode.dataType = "bin.base64";<br>
		tmpNode.nodeTypedValue = xmlHttp.responseBody;<br>
		imgBase64Data = "data:image/bmp;base64," + tmpNode.text.replace(/\n/g,"");<br>
		$("#previewImg").prop("src",imgBase64Data);<br>
	}<br>
};<br>


实现过程，点击click模拟按钮进而点击input弹出文件选择框，选择完成后调用imgChange函数<br>
