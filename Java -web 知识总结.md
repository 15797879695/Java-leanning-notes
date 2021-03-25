Java -web 知识总结

1. jsp 跟servlet 有什么区别

   **Servlet**

   - 一种服务器端的Java应用程序
   - 由 Web 容器加载和管理
   - 用于生成动态 Web 内容
   - 负责处理客户端请求

   ​     

   **Jsp**

   - 是 Servlet 的扩展，本质上还是 Servlet
   - 每个 Jsp 页面就是一个 Servlet 实例
   - Jsp 页面会被 Web 容器编译成 Servlet，Servlet 再负责响应用户请求

      

   **区别**

   - Servlet 适合动态输出 Web 数据和业务逻辑处理，对于 html 页面内容的修改非常不方便；Jsp 是在 Html 代码中嵌入 Java 代码，适合页面的显示
   - 内置对象不同，获取内置对象的方式不同

2. jsp 有哪些内置对象，有什么作用？

   1. request：对应 Java 类 javax.servlet.http.HttpServletRequest；客户端的请求信息：Http协议头信息、Cookie、请求参数等
   2. respons：对应 Java 类 javax.servlet.http.HttpServletRespons；用于服务端响应客户端请求，返回信息
   3. pageContext：对应 Java 类 javax.servlet.jsp.PageContext；页面的上下文
   4. session：对应 Java 类 javax.servlet.http.HttpSession；客户端与服务端之间的会话
   5. application：对应 Java 类 javax.servlet.ServletContext；用于获取服务端应用生命周期的信息
   6. out：对应 Java 类 javax.servlet.jsp.JspWriter；用于服务端传输内容到客户端的输出流
   7. config：对应 Java 类 javax.servlet.ServletConfig；初始化时，Jsp 引擎向 Jsp 页面传递的信息
   8. page：对应 Java 类 java.lang.Object；指向 Jsp 页面本身
   9. exception：对应 Java 类 java.lang.Throwabl；页面发生异常，产生的异常对象

3. jsp的四种作用域

   - page (当前页面作用域)：相当于 Java 关键字中 this。在这个作用域中存放的属性值，只能在当前页面中取出。对应 PageContext 类
   - request (请求作用域)：范围是从请求创建到请求消亡这段时间，一个请求可以涉及的多个页面。<jsp:forward> 和 <jsp:include> 跳转到其他页面，也在作用域范围。对应 ServletRequest 类
   - session (会话作用域)：范围是一段客户端和服务端持续连接的时间，用户在会话有效期内多次请求所涉及的页面。session 会话器，服务端为第一次建立连接的客户端分配一段有效期内的属性内存空间。对应 HttpSession 类
   - application (全局作用域)：范围是服务端Web应用启动到停止，整个Web应用中所有请求所涉及的页面。当服务器开启时，会创建一个公共内存区域，任何客户端都可以在这个公共内存区域存取值。对应 ServletContext 类

4. session跟 cookie 的区别

   浏览器和应用服务交互，一般都是通过 Http 协议交互的。Http 协议是无状态的，浏览器和服务器交互完数据，连接就会关闭，每一次的数据交互都要重新建立连接。即服务器是无法辨别每次是和哪个浏览器进行数据交互的。

   为了确定会话中的身份，就可以通过创建 session 或 cookie 进行标识。

    

   **两者区别：**

   - session 是在服务器端记录信息；cookie 是在浏览器端记录信息
   - session 保存的数据大小取决于服务器的程序设计，理论值可以做到不限；单个 cookie 保存的数据大小不超过4Kb，大多数浏览器限制一个站点最多20个cookie
   - session 可以被服务器的程序处理为 key - value 类型的任何对象；cookie 则是存在浏览器里的一段文本
   - session 由于存在服务器端，安全性高；浏览器的 cookie 可能被其他程序分析获取，所以安全性较低
   - 大量用户会话服务器端保存大量 session 对服务器资源消耗较大；信息保存在 cookie 中缓解了服务器存储用信息的压力
     

   **一般实际使用中，都是把关键信息保存在 session 里，其他信息加密保存到cookie中。**

5. session工作原理

   **1、什么是 session**
   session 是浏览器和服务器会话过程中，服务器分配的一块储存空间。服务器默认为浏览器在cookie中设置 sessionid，浏览器在向服务器请求过程中传输 cookie 包含 sessionid ，服务器根据 sessionid 获取出会话中存储的信息。

   由于 http 协议是无状态的，即 http 请求一次连接一次，数据传输完毕，连接就断开了，下次访问需要重新连接。

   通过 cookie 中的 sessionid 字段和服务器端的 session 关联，可以确定会话的身份信息。

    

   **2、session 比 cookie 更安全**
   用户信息可以通过加密存储到 cookie，但是这样做的安全性很差，浏览器的 cookie 的容易被其他程序获取和篡改。使用 session 的意义在于 session 存储在服务器，相对安全性更高。

    

   **3、session 的生命周期**

   - 创建

   浏览器访问服务器的 servlet（jsp）时，服务器会自动创建 session，并把 sessionid 通过 cookie 返回到浏览器。

   servlet 规范中，通过 request.getSession(true) 可以强制创建 session。

    

   - 销毁

   服务器会默认给 session 一个过期时间，即从该 session 的会话在有效时间内没有再被访问就会被设置过超时，需要重新建立会话。

   如 tomcat 的默认会话超时时间为30分钟。

   会话超时时间是可以通过配置文件设置，如修改 web.xml 、server.xml	

6. 浏览器禁用cookie可以使用session吗？

   一般默认情况下，在会话中，服务器存储 session 的 sessionid 是通过 cookie 存到浏览器里。

   如果浏览器禁用了 cookie，浏览器请求服务器无法携带 sessionid，服务器无法识别请求中的用户身份，session失效。

   但是可以通过其他方法在禁用 cookie 的情况下，可以继续使用session。

   - 通过url重写，把 sessionid 作为参数追加的原 url 中，后续的浏览器与服务器交互中携带 sessionid 参数。
   - 服务器的返回数据中包含 sessionid，浏览器发送请求时，携带 sessionid 参数。
   - 通过 Http 协议其他 header 字段，服务器每次返回时设置该 header 字段信息，浏览器中 js 读取该 header 字段，请求服务器时，js设置携带该 header 字段。

7. forward跟redirect的区别

   forward：转发；redirect：重定向。区别如下：

   - **浏览器 url 地址显示不同**

   服务端通过 forward 返回，浏览器 url 地址不会发生变化；服务器通过 redirect 返回，浏览器会重新请求， url 地址会发生变化

    

   - **前后台两者页面跳转的处理方式不同**

   forward 跳转页面，是服务端进行页面跳转加载（include）新页面，直接返回到浏览器；redirect 跳转页面，是服务端返回新的 url 地址，浏览器二次发出 url 请求

    

   - **参数携带情况不一样**

   forward 跳转页面，会共享请求的参数到新的页面；redirect 跳转页面，属于一次全新的 http 请求，无法共享上一次请求的参数

    

   - **http 请求次数不同**

   forward 1次；redirect 2次

    

   - **新目标地址范围不同**

   forward 必须是同一个应用内的某个资源；redirect 的新地址可以是任意地址

8. ![]()getpost 区别

9. 什么是jsonp

    定义：它是一个非官方的协议，允许在服务器端集成Script tags返回至客户端，通过 javascript callback 的形式实现跨域访问。

   特点：ajax 是通过 XmlHttpRequest 方式进行请求，JSONP 的核心是动态添加 <script> 标签来调用服务器提供的 js 脚本，两者功能很相似，实现是有区别的。
   JSONP 没有域的限制
   JSONP 的浏览器兼容性较好
   JSONP 只支持 GET 请求，只能添加超时进行异常处理
   使用 JSONP 存在 js 注入的风险

10. body中的onload事件和document.ready()有什么区别？

    - onload 表示页面包含图片等文件在内的所有元素都加载完成
    - ready 表示文档结构已经加载完成，不包含图片等非文字媒体文件

11. 写一些常见的Javascript正则表达式？

    - 身份证：/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/
    - 数字：/[0-9]/
    - 英文：/^[a-z]+$/i
    - 中文：/[\u4e00-\u9fa5]/gm
    - 数字逗号：/^[\d,]*$/
    - 手机号：/^[1][3,4,5,7,8][0-9]{9}$/

12. jQuery中有哪些选择器？

    - 基本选择器
    - 层次选择器
    - 表单选择器
    - 基本过滤选择器
    - 内容过滤选择器
    - 可见性过滤选择器
    - 属性过滤选择器
    - 子元素过滤选择器
    - 表单过滤选择器
    
13. div 居中设置跟css 内容居中设置

     div： margin：0px

     css：text-align：center

14. js 中的null 跟undefined有什么区别

     - 赋值：null 表示定义了但未赋值，undefined 表示未定义
     - 数据转换：null 在做数值转换时会被转换为 0，undefined 会被转换为 NaN

15. doget 和dopost 区别

     区别：doPost 用来处理 post 请求，doGet 用来处理 get 请求

     参数：传递的参数相同的都是 HttpServletRequest 和 HttpServletResponse

16. jQuery 的 get  跟eq方法的区别

     - get() 返回的是一个 html 对象
     - eq() 返回的是一个 jQuery 对象

17. 如何配置一个servlet?

     1.配置web.xml方法

     ```javascript
     <servlet>
      <servlet-name></servlet-name>
      <servlet-class></servlet-class>
     </servlet>
     <servlet-mapping>
      <servlet-name></servlet-name>
      <url-pattern></url-pattern>
     </servlet-mapping>
     ```

     2.注解方法

     ```javascript
     @WebServlet(name="servlet", urlPatterns={"/*"})
     ```

18. 常见的HTTP协议状态码？

     **2xx：表示请求已被成功接收、理解、接受**

     - 200(成功)  服务器已成功处理了请求。这个状态码对servlet是缺省的，如果没有调用setStatus方法的话，就会得到 200
     - 204(无内容)  服务器成功处理了请求，未返回任何内容
     - 205(重置内容) 服务器成功处理了请求，未返回任何内容，重置文档视图，如清除表单内容
     - 206(部分内容)  服务器成功处理了部分 GET 请求

     **3xx：重定向**

     - 300(多种选择)  服务器根据请求可执行多种操作。服务器可根据请求者 来选择一项操作，或提供操作列表供其选择
     - 301(永久移动)  请求的网页已被永久移动到新位置。服务器返回此响应时，会自动将请求者转到新位置
     - 302(临时移动) 服务器目前正从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。会自动将请求者转到新位置
     - 304(未修改) 自从上次请求后，请求的网页未被修改过，不会返回网页内容
     - 305(使用代理) 请求者只能使用指定的代理访问请求的网页

     **4xx：客户端错误，请求有语法错误或请求无法实现**

     - 400(错误请求) 服务器不理解请求的语法 
     - 401(身份验证错误) 此页要求授权
     - 403(禁止) 服务器拒绝请求
     - 404(未找到) 服务器找不到请求的网页
     - 406(不接受) 无法使用请求的内容特性响应请求的网页
     - 408(请求超时) 服务器等候请求时发生超时
     - 414(请求的 URI 过长) 请求的 URI 过长，服务器无法处理

     **5xx：服务器端错误，无法处理请求**

     - 500(服务器内部错误)  服务器遇到错误，无法完成请求。
     - 503(服务不可用) 目前无法使用服务器(由于超载或进行停机维护)。通常，这只是一种暂时的状态。
     - 504(网关超时)  服务器作为网关或代理，未及时从上游服务器接收请求。
     - 505(HTTP 版本不受支持) 服务器不支持请求中所使用的 HTTP 协议版本

19. servlet的生命周期

     servlet 的生命周期：

     - 初始化阶段，调用 init() 方法
     - 响应客户请求阶段，每个 servlet 请求都会调用 servlet 对象的 service() 方法，且传递请求对象 ServletRequest、响应对象 ServletResponse 参数
     - 终止阶段，调用 destroy() 方法

20. servlet的常用方法

     ​	

     ```javascript
     //初始化
     public void init(ServletConfig config) throws ServletException;
     
     //返回 servlet 初始化信息与启动参数
     public ServletConfig getServletConfig();
     
     //被 servlet 容器调用，响应 servlet 请求
     public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException;
     
     //返回 servlet 信息，如作者、版本和版权
     public String getServletInfo();
     
     //由 servlet 容器调用，把 servlet 去除
     public void destroy();
     ```

21. 过滤器的生命周期是什么样的？有什么作用？

     生命期：

     ```javascript
     //servlet 容器启动时会创建 Filter 实例
     public void init(FilterConfig filterConfig) throws ServletException;
     
     //在每次访问目标资源时执行
     public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;
     
     //服务器关闭时销毁Filter对象
     public void destroy();
     ```

     作用 :

     验证是否来自可信网络
     验证用户是否可以登录
     验证客户的浏览器版本
     对提交的数据进行重新编码
     过滤敏感词汇
     记录系统日志

22. jquery书写ajax的的方式及参数说明

     ```javascript
     $.ajax({
         dataType : 'json',
         url : 'http://localhost:8080/data.do',
         data : '{"id":1}',
         type : 'POST',
         success : function(ret) {
             console.log(ret);
         },
         error : function(ret) {
             console.log(ret);
         }
     });
     ```

     ```javascript
     
     url          String 类型的参数，发送请求的地址
     data         Object 或 String 类型，发送到服务器的数据
     type         String 类型，请求方式get或post
     datatype     String 类型，预期服务器返回的类型
     timeout      number 类型，设置请求超时时间
     async        boolean 类型，异步为 true(默认)，同步为 false
     cache        boolean 类型，默认为true，是否从浏览器缓存中加载信息
     beforesend   Function 类型，如添加自定义 http header
     ```

23. JSP常用的标签

     - 请求转发：<jsp:forward>
     - 页面传递数据：<jsp:param>
     - 输出标签：<c:out>
     - 判读标签<c:if>
     - 迭代标签<c:foreach>
     - 多重判断标签<c:choose>

24. 如何防止表单重复提交？

     解决办法:

     - 数据库主键唯一
     - 提交成功后页面重定向
     - 按钮提交后隐藏或不可再点击
     - 后台生成页面 token，页面表单提交携带 token，后台进行校验

25. 说说Tomcat的模块架构

     Tomcat 是一个 Web 容器，包含 HTTP 服务器 + Servlet 容器。
      
     **Web 容器的两个核心功能：**

     - 处理 Socket 连接，处理网络字节流与 Request 和 Response 对象的转化
     - 加载和管理 Servlet，处理 Request 请求

     Tomcat 的两个核心组件连接器（Connector）和容器（Container）来分别做这两件事情。连接器负责对外连接，容器负责内部对请求的处理。

     
     **Tomcat 的核心模块：**

     - Server：Catalina Servlet 容器。Tomcat 提供了 Server 接口的一个默认实现，通常不需要用户自己去实现。在 Server 容器中，可以包含一个或多个 Service 组件。
     - Service：存活在 Server 内部的中间组件，它将一个或多个连接器（Connector）组件绑定到一个单独的引擎（Engine）上。Service 也很少由用户定制，Tomcat 也提供了 Service 接口的默认实现。
     - Connector：连接器，处理与客户端的通信，它负责接收客户请求，以及向客户返回响应结果。在 Tomcat 中，有多个连接器可以使用。
     - Engine：Servlet 引擎，表示一个特定的 Service 的请求处理流水线，从连接器接收和处理所有的请求，将响应返回给适合的连接器，通过连接器传输给用户。在 Tomcat 中，每个 Service 只能包含一个 Engine。可以通过实现 Engine 接口提供自定义的引擎。
     - Host：一个虚拟主机，一个引擎可以包含多个 Host。Tomcat 给出了 Host 接口的默认实现 StandardHost。
     - Context：一个 Web 应用程序，运行在特定的虚拟主机中。一个 Host 可以包含多个Context，每个 Context 都有一个唯一的路径。通常不需要创建自定义的 Context，Tomcat 给出了 Context 接口的默认实现  StandardContext。



