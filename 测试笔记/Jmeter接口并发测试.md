# Jmeter接口并发测试
## 详细步骤
##### １、在测试计划中添加线程组
![图一](http://img.shanrufu.com/jmeter_login_test_setp_01.png)
##### 2、配置线程组，设置线程数，Ramp-Up Period(准备时间)，循环次数
![图二](http://img.shanrufu.com/jmeter_login_test_setp_02.png)
##### 3、添加一个同步定时器，同一时刻向服务器发起请求
![图三](http://img.shanrufu.com/jmeter_login_test_setp_03.png)
##### 4、配置同步定时器，设置同一时刻发起请求的线程数
![图四](http://img.shanrufu.com/jmeter_login_test_setp_04.png)
##### 5、添加一个数据配置元件
![图五](http://img.shanrufu.com/jmeter_login_test_setp_05.png)
##### 6、打开WPS生成一定数量的随机数做为登录账号密码，文件格式另存为CSV格式
![](http://img.shanrufu.com/jmeter_login_test_setp_06.png)
##### 7、配置数据来源，字符集，以及读取时的参数名（这里为account，password），其他保持默认即可
![](http://img.shanrufu.com/jmeter_login_test_setp_07.png)
##### 8、添加一个察看结果树和一个聚合报告
![](http://img.shanrufu.com/jmeter_login_test_setp_08.png)
##### 9、最后添加一个HTTP请求元件
![](http://img.shanrufu.com/jmeter_login_test_setp_09.png)
##### 10、通过Jmeter录制脚本结果，我们可以分析得到测试登录接口的路径，以及服务器端口，因为待测试的接口访问路径需要一个13位的时间戳的参数，下图中timestamp参数，所以我们需要添加一个获取时间戳的内置函数
![](http://img.shanrufu.com/jmeter_script_record_setp_15.png)
##### 11、菜单栏选项，选择函数对话框，打开函数助手
![](http://img.shanrufu.com/jmeter_login_test_setp_10.png)
##### 12、因为我们需要的是一个获取时间戳的内置函数，功能选择_ _time；再点击生成按钮，复制左侧输入框中生成的字符串
![](http://img.shanrufu.com/jmeter_login_test_setp_11.png)
##### 13、配置**HTTP请求**，接口访问协议、服务器IP、访问端口、请求方法、接口路径，接口路径最后粘贴上我们刚刚复制的获取时间戳的内置函数，因为我们通过脚本录制抓包得到的结果显示，登录接口输入的账号密码是放在HTTP请求体中以JSON格式发送给服务端的，所以我们配置Body Data中这个JSON数据

```json
{"account":"${account},"password":"${password}"}
${account}、${password}:步骤7配置数据源设置的读取我们CSV文件时的参数名
```
![](http://img.shanrufu.com/jmeter_login_test_setp_12.png)
##### 14、点击开始，启动测试，在聚合报告中查看总体测试结果
![](http://img.shanrufu.com/jmeter_login_test_setp_13.png)
##### 15、点击察看结果树，可以查看单个HTTP请求的请求信息以及服务器的响应数据
![](http://img.shanrufu.com/jmeter_login_test_setp_14.png)

## 总结
- 在测试计划中添加线程组，并配置线程数量、准备时间以及循环次数
- 在线程组下添加同步定时器，设置所有线程同一时刻访问测试接口
- 同级添加一个CSV Data Set Config测试元件，配置测试数据的来源，以及使用时的参数名称
- 同级添加聚合报告、察看结果树，用于统计分析
- 最后添加一个HTTP请求元件，配置测试接口的访问协议，服务器IP，后端访问端口，访问协议，访问路径，并填写上获取时间戳的内置函数
- 设置HTTP请求信息，如果以表单形式提交数据，则在Paraemers中配置表单参数；如果数据是放在HTTP请求体中，则配置Body datas（该教程示例中是将数据以JSON格式存放在HTTP请求体中发送给服务器）
- 点击开始，启动测试，查看聚合报告以及查看结果，聚合报告得到总体测试结果，察看结果可得到每个HTTP请求的详细信息

## 注意事项
1. 测试接口并发能力一定要添加同步定时器，让线程组所有线程同一时刻访问服务器
2. 在HTTP请求测试元件中使用的参数一定要和CSV Data Set Config配置的一样，否则无法正确读取本地文件的数据，该示例中为account，password，使用时为${account}${password}
3. HTTP请求测试元件中的端口为后端端口，访问路径一定要正确