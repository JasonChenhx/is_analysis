@startuml
# 实验5：图书管理系统数据库设计与界面设计
|学号|班级|姓名|照片|
|:-------:|:-------------: | :----------:|:---:|
|2015104144201|软件(本)15-4|陈惠翔|![flow1](../chx1.jpg)|

## 1.数据库表设计

## 1.1. Book表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|ISBN|varchar2(14)|主键|否| | | 书本印刷的ISBN,且每本书都不一样|
|book_name|varchar2(60)| |否||||
|author|varchar2(50)| |是||||
|publisher|varchar2(100)| |是||||
|price|double| |否|0.0|||
|public_date|date| |否|||出版日期|
|total_Num|int| |否|0| |即该图书库存的总量|
|rest_Num|int| |否|0| |目前可借的图书量|
|is_borrow|int| |否|0| |是否可借|

## 1.2. Reader表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|user_ID|varchar2(32)|主键|否|||用户ID创建的时候自增|
|user_name|varchar2(32)| |否| | ||
|gender|int|| ||否| |0||性别(0男1女)|
|password|varchar2(32)| |否| '123456'| | 密码采用32位MD5加密|
|college|varchar2(50)| |是| | |该读者所在学院|
|borror_number|int| |是| 0| |已借数量|


## 1.3. Administrator表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|user_ID|varchar2(32)|主键|否||||
|user_name|varchar2(32)| |否| '管理员'|||
|gender|int|| ||否| |0||性别(0男1女)|
|password|varchar2(32)| |否| '123456'| |密码采用32位MD5加密|


## 1.4. LendRecord表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|lendrecord_recordID|varchar2(32)|主键|否| | |借书编号|
|user_ID|varchar2(32)|外键|否| |reader.userID||
|ISBN|varchar2(14)|外键|否| |Book.ISBN||
|borrow_date|datetime| |否| | |借阅时间|
|is_renew|tinyint| |否|0| 0-1|是否续借图书，1为已续借，0为未续借|
|return_date|datetime| |否| | |归还时间，若为空代表未归还|

## 1.5. Forfeit表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|forfeit_id|int|主键|否| | |逾期缴费ID自增|
|user_ID|varchar2(32)||否| ||读者编号|
|book_id|varchar2(32)| |否| | |书籍名称|
|book_name|varchar2(10)| |否| | |书籍名称|
|borrow_date|datetime| |否| | |借阅时间|
|return_date|datetime| |否| | |归还时间，若为空代表未归还|
|forfeit_money|double||否|0.0||逾期金额|



## 1.6. ManageBookRecord表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|manageBookRecordID|varchar2(32)|主键|否| | ||
|user_ID|varchar2(32)|外键|否| |Administrator.userID||
|ISBN|varchar2(14)|外键|否| |Book.ISBN||
|manageDate|datetime| |否| | |管理图书的时间|
|action|varchar2(10)| |否| | |管理图书的动作，add添加图书，delete为删除图书，update为更新图书信息|
|manegeNum|int| |否| | |管理图书的数量|


## 1.7. BookReserve表
|字段|类型|主键，外键|可以为空|默认值|约束|说明|
|:-------:|:-------------:|:------:|:----:|:---:|:----:|:-----|
|bookrecord_recordID|varchar2(32)|主键|否| | ||
|userID|varchar2(32)|外键|否| |Reader.userID||
|ISBN|varchar2(14)|外键|否| |Book.ISBN||
|book_Date|datetime| |否| | |预定图书的时间|
|active_Time|int| |否|7| |预定图书的有效期，单位为天|
|is_cannel|int| |否|0| 0-1|此预订是否取消，1表示已取消，0表示未取消|
***

## 2. 界面设计
## 2.1. 图书归还界面设计
![pic1](pic.PNG)
- 用例图参见：图书归还用例
- 类图参见：与Book类相关的
- 顺序图参见：图书归还顺序图
- API接口如下：

1. 获取用户未归还图书API

- 功能：用于获取某用户全部未归还图书
- 请求地址： http://localhost/v1/api/get_unreturnbooks_4reader
- 请求方法：POST
- 请求参数：

|参数名称|必填|说明|
|:-------:|:-------------: | :----------:|
|user_ID|是|根据读者编号获取其未归还图书列表 |

- 返回实例：
```
{
	"is_unreturnbooks_readers": [{
		"ISDN": "978-7-5605-3454-1",
		"book_name": "线性代数辅导讲义",
		"author": "李永乐",
		"publisher":"西安交通大学出版社"
		"price":"49.80"
		"public_date":"2010-2-1"
		"total_num":"10"
		"rest_num":"3"
		"is_borrow":"是"

	}, {
        "ISDN": "978-7-5022-8668-2",
               		"book_name": "高等数学辅导讲义",
               		"author": "汤家凤",
               		"publisher":"中国原子能出版社"
               		"price":"38.00"
               		"public_date":"20017-11-1"
               		"total_num":"8"
               		"rest_num":"3"
               		"is_borrow":"是"
	},{
	                    "ISDN": "978-7-04-039661-2",
                   		"book_name": "工程数学线性代数",
                   		"author": "同济大学数学系",
                   		"publisher":"高等教育出版社"
                   		"price":"19.40"
                   		"public_date":"1981-11-1"
                   		"total_num":"8"
                   		"rest_num":"1"
                   		"is_borrow":"是"
	} ,{
        "ISDN": "978-7-5022-8870-9",
        		"book_name": "接力题典1800",
        		"author": "汤家凤",
        		"publisher":"中国原子能出版社"
        		"price":"68.00"
        		"public_date":"20018-2-1"
        		"total_num":"15"
        		"rest_num":"0"
        		"is_borrow":"否"
	}],
    "readerLendBook": {
        "user_ID": "201510416603",
        "user_name": "高等数学",
        "profession": "考研",
        "grade": "2015级"
    },
	"length": "4",
	"status": "1"
}
```
- 返回参数说明：

|参数名称|说明|
|:-------:|:-------------: |
|is_unreturnbooks_reader|该用户未归还图书的集合|
|readerLendBook|该读者的用户信息|
|length|未归还图书数量|
|status|获取结果(1:succeed 0:failed)|

2. 归还图书API
- 功能：用于向系统提交需要归还的图书项目
- 请求地址： http://localhost/v1/api/return_books
- 请求方法：POST
- 请求参数：

|参数名称|必填|说明|
|:-------:|:-------------: | :----------:|
|userID|是|根据读者编号归还其借阅图书 |
|ISDN|是|待归还图书的ISBN号 |

- 返回实例：
```
{
	"length": "3",
	"status": "1"
}
```
- 返回参数说明：

|参数名称|说明|
|:-------:|:-------------: |
|length|成功归还图书数量|
|status|获取执行结果(1:succeed 0:failed)|



@enduml
