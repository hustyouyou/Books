一、一些要交代的话      
此篇文章写于初识Java之出，由于接触Java web时间比较短，代码的价值并不是很高，只是基础的一些东西。
数据库采用的是Mysql，简单实现图书的借取与归还，服务器采用的是Tomcat，环境自行配置。Demo的下载地址 
二、具体的实施
1.创库建表初始化一些数据。效果如下图




数据库表结构

2.创建工程结构，如图




工程结构

com.test包下放Java后台文件，css html js 文件夹各放置对应的文件，lib放置工程所需的jar包。
3.构建第一个界面，登录界面。      
登录界面采用的是，用户无需账号密码即可登录，管理员需要输入预设值的账号密码进行登录，界面分别跳转到相应的界面。




登录界面
         以下html代码




Log.html主要代码

            以下为Log.js代码




Log.js代码
       利用angularjs的双向数据绑定进行获取输入框的输入内容，window.location.href="文件路径"进行页面跳转。
       3.用户界面，具有查询、刷新、借书及还书功能。




用户界面user.html效果图

       查询、刷新、借书及还书均通过http传输协议进行前后端的数据传递，借书还书时前端往后端传送书名，
       后端进行数据库查询，判断书的状态，进行相应的操作，执行相应的响应。需要在web.xml文件中进行服务器的配置。




web.xml配置
获取数据库全部数据进行GUI的显示。执行过程：       user.js发送http协议访问http://localhost:8080/BookManageSystem/run,run为映射名，
找到映射后对应的找到了相应的servlet名，进而找到了文件所在具体位置为com.test下的名为Test的文件。Test文件下代码




Test.java代码
       其中Group为一具有name,author,type,hava属性的一个类，执行http请求后，链接数据库进行查询获取结果集，
       遍历结果集，将结果集内容打包成Group对象放入数组list中，随后将数组写成Json类型 out.write(string);返回Json数据。
       这里面转化Json数据用到的是阿里巴巴的fastjson，需要自行下载jar包放入lib文件夹及WEB-INF下的lib文件夹中，
       b别忘了build-path!




WEB-INF下lib文件夹
此时我们拿到了数据库里的数据，返回了一个Json数组。看下user.js代码




user.js代码
返回值赋值给自己作用域的names，html内通过遍历names完成页面数据的填充。




user.html遍历数据赋值的代码
4.带参数的http请求。我们看下管理员添加图书的逻辑吧。




manager.html
       需要通过http传送三个参数进行数据的添加。当然第一步还是要配置servlet.在web.xml文件里添加以下代码




web.xml

配置完成后再来到manager.js文件下




manager.js文件内添加书籍代码

        其中postData变量为要传递的参数及参数值，随后拼接到http://localhost:8080/BookManageSystem/add后面。
        此时我们再来带http请求的位置Add.java文件内




获取传递过来的参数
        获取传递过来的参数并进行转码（避免乱码）。随后进行数据库的操作。
        
三、一些总结
 到这里逻辑大致清楚了,通过http前端访问后台数据接口需要配置servlet设置映射，通过访问映射间接访问接口所在类，
 完成相应操作，返回数据通过PrintWriter out = response.getWriter();out.write("返回的数据");完成。 当然
 这里仅仅是演示了前后端通信的逻辑，相关的功能可自行增加完成。这里提供Demo的下载地址，仅供参考。点我下载Demo

作者：雪之下丶微凉
链接：https://www.jianshu.com/p/6f21738e4b7b
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
