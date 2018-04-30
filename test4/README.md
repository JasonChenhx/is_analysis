# 实验4：图书管理系统顺序图绘制
|学号|班级|姓名|照片|
|:-------:|:-------------: | :----------:|:---:|
|201510414401|软件(本)15-4|陈惠翔|![flow1](../chx.jpg)|

## 图书管理系统的顺序图

## 1. 借阅图书用例
## 1.1. 借阅图书用例PlantUML源码

``` sequence
@startuml
actor 图书管理员
participant "登陆系统"
participant "图书记录"
participant "借阅记录"
actor "借阅者"

图书管理员 -> 登陆系统: 1:验证身份
activate 图书管理员
activate 登陆系统
登陆系统-->图书管理员: 2: 返回信息
deactivate 登陆系统

借阅者 -->图书管理员::                                                3:发出借书请求
activate 借阅者
deactivate 借阅者


图书管理员 -> 图书记录::           4:扫描书籍ID
activate 图书记录
deactivate 图书管理员

图书记录-->图书管理员::      5:返回该书籍信息
activate 图书管理员
deactivate 图书管理员

图书记录->图书记录:6:记录书籍借出
图书记录-->图书管理员::         6:借阅成功
deactivate 图书记录

activate 图书管理员
图书管理员 ->借阅者::                                      7:将书给借阅者
deactivate 图书管理员
@enduml
```

## 1.2. 借阅图书用例顺序图
![class](test4-1.png)

## 1.3. 借阅用例顺序图说明
1、图书管理员登陆系统2、借阅者发起借书请求3、图书管理员扫描借阅书籍的ID4、图书记录返回书籍信息 5、向借阅者返回操作信息

***

## 2. 还书用例
## 2.1. 还书用例PlantUML源码

``` sequence
@startuml
actor 图书管理员
participant "登录系统"
participant "图书记录"
participant "借阅记录"
actor "借阅者"

图书管理员 -> 登录系统: 1:验证身份
activate 图书管理员
activate 登录系统
登录系统-->图书管理员: 2: 返回信息
deactivate 登录系统

借阅者-->图书管理员::                                       3:发出还书请求
activate 借阅者
deactivate 借阅者
deactivate 图书管理员

图书管理员 -> 借阅记录::                        4:扫描书籍ID
activate 图书管理员
deactivate 图书管理员

activate 借阅记录
借阅记录->借阅记录:5:记录借阅者还书
借阅记录 -->图书管理员::                       6:返回读者和书籍信息
deactivate 借阅记录

activate 图书记录
activate 图书管理员
deactivate 图书管理员

图书记录->图书记录:7:记录书籍归还

图书记录-->图书管理员::         8:还书成功

deactivate 图书记录
@enduml
```

## 2.2. 还书用例顺序图
![class](test4-2.png)

## 2.3. 还书用例顺序图说明
1、图书管理员登陆系统 2、借阅者发起还书请求 3、图书管理员扫描该书籍的ID 4、向借阅记录返回读者和书籍信息 5、记录书籍归还 6、向借阅者显示还书成功操作信息
***

## 3. 预定图书用例
## 3.1. 预定图书用例PlantUML源码

``` sequence
@startuml
actor 图书管理员
participant "登录系统"
participant "图书记录"
participant "借阅记录"
actor "借阅者"
图书管理员 -> 登录系统: 1:验证身份
activate 图书管理员
activate 登录系统
登录系统-->图书管理员: 2: 返回信息
deactivate 登录系统

借阅者 -->图书管理员::                                       3:发出预定图书请求
activate 借阅者
deactivate 借阅者
deactivate 图书管理员

图书管理员->图书记录:: 4:是否该图书满足借书要求
activate 图书管理员
activate 图书记录
图书管理员<-- 图书记录:: 5:返回图书满足预定要求
deactivate 图书记录
deactivate 图书管理员

图书管理员->借阅者 ::                        6:预定图书成功，在规定时间取书
activate 图书管理员
activate 借阅者
deactivate 图书管理员
deactivate 借阅者



@enduml
```

## 3.2. 预定图书用例顺序图
![class](test4-3.png)

## 3.3. 预定图书用例顺序图说明
1、图书管理员登陆系统 2、借阅者发起预定图书请求 3、图书管理员向图书记录查询是否满足借书要求 4、若满足，向借阅者返回操作结果，并提示按规定时间取书

***

## 4. 缴纳罚金用例
## 4.1. 缴纳罚金用例PlantUML源码

``` sequence
@startuml
actor 图书管理员
participant "登录系统"
participant "图书记录"
participant "借阅记录"
actor "借阅者"

图书管理员 -> 登录系统: 1:验证身份
activate 图书管理员
activate 登录系统
登录系统-->图书管理员: 2: 返回信息
deactivate 登录系统
deactivate 图书管理员

图书管理员->借阅者::                              3:获取借阅者信息
activate 图书管理员
activate 借阅者
deactivate 借阅者

图书管理员->借阅记录 ::                        4:获取超期时间
activate 借阅记录
借阅记录->借阅记录:: 5:计算超期时间，并转换为罚金
deactivate 图书管理员
deactivate 借阅记录

借阅者-->图书管理员 :: 6:缴纳相应罚金
activate 图书管理员
activate 借阅者
deactivate 图书管理员
deactivate 借阅者

@enduml
```

## 4.2. 缴纳罚金用例顺序图
![class](test4-4.png)

## 4.3. 缴纳罚金用例顺序图说明
1、图书管理员登陆系统 2、获取借阅者信息 3、想借阅记录获取超时时间 4、向借阅者反馈超时时间，并通知按时交纳罚金


***

## 5. 管理图书用例
## 5.1. 管理图书用例PlantUML源码

``` sequence
@startuml
actor 图书管理员
participant "登录系统"
participant "书籍"
participant "图书记录"
participant "借阅记录"
actor "借阅者"

图书管理员 -> 登录系统: 1:验证身份
activate 图书管理员
activate 登录系统
登录系统-->图书管理员: 2: 返回信息
deactivate 登录系统
deactivate 图书管理员

图书管理员-> 书籍::          3:管理书籍
activate 图书管理员
activate 书籍
deactivate 图书管理员
书籍-> 图书记录 ::  4:保存本次管理记录
deactivate 书籍
activate 图书记录
图书记录--> 图书管理员 ::                  5:管理成功返回操作结果
activate 图书管理员
deactivate 图书记录
deactivate 图书管理员
@enduml
```

## .52. 管理图书用例顺序图
![class](test4-5.png)

## 5.3. 管理图书用例顺序图说明
1、图书管理员登陆系统 2、图书管理员管理书籍（对书籍的增删改查） 3、向图书记录发送并保存本次管理记录
***
