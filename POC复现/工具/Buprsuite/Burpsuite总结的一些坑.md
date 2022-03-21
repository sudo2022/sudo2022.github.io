# 0x01 burpsuite 多线程爆破
* 我们在经常使用`BP`的时，时长会拿这款工具做爆破功能，但是有时候我们在自己靶场玩的时候，就会发现，自己所有的配置都是对的，字典里也确实存在正确的密码，但是从`BP`的结果上并不能发现正常的爆破结果。其实这有一部分原因是由于`BP`线程设置的多高，导致数据混乱没有拿到正确的返回数据。
* 以下是我遇坑的过程（以`dvwa`线下靶场为例子）：
    * 正常抓包
    ![图 1](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-194712637.png)  
    * 将`post`请求提交到`intruder`，准备进行爆破
    ![图 2](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-194907537.png)  
    * 进行`payload`设定
    ![图 3](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195018218.png)  
    ![图 4](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195107690.png)  
    ![图 5](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195211108.png)  
    * 配置`token获取`
    ![图 6](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195253622.png)  
    ![图 7](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195336316.png)  
    ![图 8](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195404948.png)  
    ![图 9](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195430952.png)  
    ![图 10](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195458848.png)  
    ![图 11](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195528706.png)  
    ![图 12](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195632137.png) 
    ![图 13](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195720005.png)  
    ![图 14](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-195956388.png)  
    * 修改线程数量，开始爆破尝试，这次我选择线程为1
    ![图 16](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-200159565.png)  


# 0x02 burpsuite 抓取不到youtube等一些做了防护的网站
* 当我们在渗透测试的过程中，也会遇到问题，比如：我明明已经配置了`BP`的证书，抓取其他所有`HTTP`流量，都是正常，但是总有个别网站在抓取流量的时候发现报错未能响应`XXXXX:443`等错误，然后就开始一直捣鼓证书的问题，其实不然，是对方网站做了一些安全防护
* 这里我们就需要想绕过这些安全防护，这里有个思路：
    ![图 17](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-200937992.png)  
* 实验测试：
    * 正常访问`youtube`，这里只是实验，切勿做违法事情
    ![图 18](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201106321.png)  
    * 开始代理监听`8080`，在漫长的等待后，结果是出人意料的进不去
    ![图 19](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201346593.png)  
    ![图 20](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201445700.png)  
    * 添加一层代理，使用：`Fiddler Classic`
    ![图 21](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201539148.png)  
    * `BP`进行配置
    ![图 22](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201627077.png)  
    ![图 23](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201647366.png)  
    ![图 24](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201659367.png)  
    * 重新再次抓包
    ![图 25](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201748401.png)  
    ![图 26](.images/Burpsuite%E6%80%BB%E7%BB%93%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/IMG_20220124-201836449.png)  
