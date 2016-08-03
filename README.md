# Development-skills
document  
**1. imag.js里有哪些标准JavaScript对象？**  
imag.js里的标准JavaScript对象有Object,Object, Function, Array, Boolean, Date, Math, Number, String, RegExp, Global Functions, JSON。    
**2.为什么客户端会提示XML语法错误？**  
imag.js的代码文档遵循严格的XML语法规范，开发时要注意以下地方：  
1. label, script, web等标签的text可能含有XML特殊符号<、&， 注意使用CDATA来标记。  
2. 属性内含有特殊符号&需要用&amp;来转义，如：href="nextpage.jsp?username=Terry&amp;password=123"  
3. 属性之间不能缺少空格，也就是两个属性不能写连着。如：<button style="color:red"onclick="alert(1);">按钮</button>，其中style和onclick连着了。  
4. 属性不能重复，如果如：<button style="color:red" style="background:blue;">按钮</button>，其中定义了两个style属性。  
5. CDATA里不能再包含有CDATA，如：<web><![CDATA[<script><![CDATA[alert(0);]]></script>]]></web>，其中CDATA进行了嵌套会出错。
注意：如果代码在Android系统上没问题，在iOS系统上提示XML语法错误，请检查上面3,4两种情况。    
**3.在JS脚本里如何进行CDATA的嵌套？**    
有时候需要在JavaScript里进行CDATA的嵌套，但这样会报XML语法错误，如：  
```xml
<script>
<![CDATA[
    $page.onload = function() {   
        var label = $C('<label><![CDATA[文本&内容]]></label>');
        $('row').add(label);
    };
]]>
</script> 
```
此时可以调用一个拼字符串生成CDATA的JS方法，从而避免直接使用CDATA，如：  
```
<script>
<![CDATA[
    function cdata(text) {
        return '<![' + 'CDATA[' + text + ']]' + '>';
    }
         
    $page.onload = function() {
        var label = $C('<label>' + cdata('文本&内容') + '</label>');
        $('row').add(label);
    };
]]>
</script>
```
**4.如何引入公共的JS文件？**  
使用page的include属性，可以引入多个JS文件，具体请参考：[引入JS文件](http://www.imagapp.com/doc/page#include)  
**5.如何在页面之间传递参数？**  
页面之间传递参数有两种方式：  
一种方式是用URL传递参数，如：$page.open('nextpage.xml?username=Terry')，具体请参考[传递参数](http://www.imagapp.com/doc/develop#page_param)。  
另一种方式是用$phone.sessionStorage()或者$phone.localStorage()方法，具体请参考[离线存储](http://www.imagapp.com/doc/phone-storage)。  
**6.如何记住登录用户名和密码？**  
有三种方式:  
1使用表单控件的remember属性。  
2使用$phone.localStorage()保存数据。  
3在服务器后台设置Cookie保存表单数据，Cookie会保存到手机客户端。  
**7.如何实现自动登录功能？**  
在服务器端设置用户登录之后的参数到客户端的Cookie里，imag.js客户端在访问服务器时会自动带上Cookie信息，然后服务器向客户端返回登录成功信息。具体做法和PC端浏览器实现自动登录的方式一致。  
**8.如何访问后台数据库？**  
在imag.js中访问后台数据库有两种方式，一种是通过后台程序读取数据库数据，再用JSP，ASP等脚本输出imag.js标签，这种方式类似于动态网页。另一种方式是通过[$http.get()](http://www.imagapp.com/doc/http)和[$http.post()](http://www.imagapp.com/doc/http)方法来获取服务器端的数据，这种方式类似于Ajax。具体请参考：imag.js客户端访问后台数据库的两种方式 ，关于开发环境的搭建请参考产品帮助：[本地开发和调试](http://www.imagapp.com/doc/envjava)。  
**9.如何设置页面定时器？**  
设置定时器使用$page.setTimeout()和$page.setInterval()方法，具体参考：[页面方法](http://www.imagapp.com/doc/page)。  
**10.如何使用画廊浏览一组图片？**  
画廊功能使用$page.gallery()方法，具体参考：[画廊方法](http://www.imagapp.com/doc/page#gallery)。  
**11.如何展示树形结构？**    
展示树形结构使用可折叠的列表，需设置ListItem的collapsed属性，具体参考：[可折叠的列表](http://www.imagapp.com/doc/list#list_collapsed)。  
**12.如何使用索引排序列表？**  
使用索引排序列表需设置list的reuse="sort"，具体参考：[索引排序列表](http://www.imagapp.com/doc/list#list_sort)。  
**13.如何实现向上分页功能？**  
先用list的scrollToBottom()方法将滚动条定位到底部，然后用addTopMore()方法向列表顶部添加数据。  
**14.如何获取IMEI, IMSI, MAC地址等手机信息？**  
通过$phone.info()方法来获取IMEI, IMSI, MAC地址等手机信息，具体参考：[客户端信息](http://www.imagapp.com/doc/phone-info)。  
**15.如何限定手机运营商？**  
通过$phone.info()['operator']来获取运营商信息并进行控制，具体参考：[限定运营商](http://www.imagapp.com/doc/phone-info#operator)。  
**16.如何控制Android和iOS平台分别执行不同的代码？**    
通过$phone.info()方法获取platform参数，然后用if else条件语句控制在不同平台分别执行不同代码。  
