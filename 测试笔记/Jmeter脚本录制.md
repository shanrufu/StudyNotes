# Jmeter脚本录制

## 启动Jmeter
1. [Ctrl + Alt + T ]打开命令行 输入 [sudo su] 切换到root用户下
`sudo su`
2. 输入java -version，查看是否配置JDK，如未配置参考：
`java -version`
3. 输入cd命令，切换到Jmeter存放的bin目录下
`cd 自己Jmeter存放路径`
4. 输入./jmeter.sh，启动Jmeter
`./jmeter.sh`
5. ![启动Jmeter最终结果](http://img.shanrufu.com/jmeter_script_record_setp_00.png)

## Jmeter录制脚本
##### 1、在测试计划下添加线程组，不需要配置线程数，保持默认配置
![](http://img.shanrufu.com/jmeter_script_record_setp_01.png)  
##### 2、在线程组上鼠标右键添加，在逻辑控制器下选择录制控制器，添加一个录制控制器
![](http://img.shanrufu.com/jmeter_script_record_setp_02.png)  
![](http://img.shanrufu.com/jmeter_script_record_setp_03.png)  
##### 3、在录制控制器上鼠标右键，在配置元件下选择Http请求默认值，添加一个Http请求默认值元件
![](http://img.shanrufu.com/jmeter_script_record_setp_04.png)  
##### 4、设置需要测试接口的网络协议（一般为Http或者Https），设置需要测试接口的域名或者网络IP，如果测试的接口使用的不是默认的80端口，则还LaunchTime需设置端口号，反之可不写
![](http://img.shanrufu.com/jmeter_script_record_setp_05.png)  
##### 5、继续在录制控制器上鼠标右键，在监听器下选择察看结果树，添加一个察看结果树
![](http://img.shanrufu.com/jmeter_script_record_setp_06.png)  
![](http://img.shanrufu.com/jmeter_script_record_setp_07.png)  
##### 6、在工作台上鼠标右键，在非测试元件下选择HTTP代理服务器，添加一个代理服务器
![](http://img.shanrufu.com/jmeter_script_record_setp_08.png)  
![](http://img.shanrufu.com/jmeter_script_record_setp_09.png)  
##### 7、配置代理服务器，打开系统设置选择网络配置，网络代理选择手动，已本机做为Jmeter的代理服务器，IP输入127.0.0.1，端口设置为一个不常用端口，**不需要和测试接口的端口一样**
![](http://img.shanrufu.com/jmeter_script_record_setp_10.png)  
##### 8、 打开火狐浏览器，首选项，网络设置，选择使用系统代理；***特别注意：在Jmeter没有启动测试前，无法访问网络，会显示代理服务器错误***
  ![](http://img.shanrufu.com/jmeter_script_record_setp_11.png)  
##### 9、添加过滤条件，过滤掉图片等无用文件  
![](http://img.shanrufu.com/jmeter_script_record_setp_12.png)  
##### 10、点击启动，忽略提示窗口，**打开浏览器，访问需要测试界面**
![](http://img.shanrufu.com/jmeter_script_record_setp_13.png)  
##### 11、在右侧的文件树下可以看到录制结果  
![](http://img.shanrufu.com/jmeter_script_record_setp_14.png)  
##### 12、选择需要测试的接口，这里以登录接口为例，查看浏览器发送的HTTP请求体数据，得到接口参数信息
![](http://img.shanrufu.com/jmeter_script_record_setp_15.png)
##### 13、在系统设置中关闭网络代理（一定要关闭，一定要关闭，一定要关闭）
![](http://img.shanrufu.com/jmeter_script_record_setp_16.png)

## 总结
- 以**root用户**启动Jmeter
- 在**测试计划**下添加**线程组**
- 在**线程组**下添加**录制控制器**
- 在**录制控制器**下添加**HTTP默认参数**，配置请求测试接口的**访问协议**，**访问IP**，**访问端口**
- 在**录制控制器**下添加**察看结果树**
- 在**工作台下**添加**HTTP代理服务器**，配置代理服务器的IP和端口，以及添加结果过滤条件
- 打开**系统设置**，在**网络设置**中手动**配置代理服务器的IP和端口**（IP：127.0.0.1，PORT：8080）
- 打开浏览器，在设置或首选项中配置浏览器的代理服务器为使用**系统代理服务器**
- 在Jmeter中**启动代理服务器**，**开始录制脚本**，在浏览器中访问需要测试的页面或接口
- 在左侧中结果中找到需要测试的接口，查看请求体，请求头，请求方式，获取需要的信息
- 在**系统设置中关闭代理服务器**

## 特别注意
1. Jmeter一定**以root用户启动**，否则会报权限不够的错误
2. 代理服务器**IP为127.0.0.1**，并设置**浏览器使用系统代理**
3. 一定要**先启动Jmeter脚本录制**，**再访问页面**，否则浏览器显示代理服务器错误
4. 录制结束后一定要在系统设置中**关闭代理服务器**

