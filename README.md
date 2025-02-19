# Autoticket
大麦网自动抢票工具

讨论QQ群：680269358

## Preliminary
Python 3.6+

Option1：Firefox（测试版本：v68.0.1.7137） + geckodriver（测试版本：v0.24.0）

Option2：Chrome （测试版本：v77.0.3865.90） + Chrome driver （测试版本：v77.0.3865.10）

## Set up
pip install selenium

注：支持Windows、Linux、MacOS，请移步Wiki更换浏览器驱动

## Basic usage
在config.json中输入相应配置信息，具体说明如下：

{
    
    "sess": [ # 场次优先级列表，如本例中共有三个场次，根据下表，则优先选择1，再选择2，最后选择3；也可以仅设置1个
        1,
        2,
        3,
    ],
    "price": [ # 票价优先级，如本例中共有三档票价，根据下表，则优先选择1，再选择3；也可以仅设置1个
        1,
        3
    ],
    "date": 0, # 选择第几个日期，默认为0表示不选择
    "real_name": [# 实名者序号，如本例中共有两位实名者，根据序号，同时选择1，2位实名者，留空表示无需实名购票
        1,
        2
    ],
    "nick_name": "your_nick_name", # 用户的昵称，用于验证登录是否成功
    "ticket_num": 2, # 购买票数
    "damai_url": "https://www.damai.cn/", # 大麦网官网网址
    "target_url": "https://detail.damai.cn/item.htm?id=599834886497", # 目标购票网址
    "browser": 0 # 浏览器类别，0为Chrome（默认），1为Firefox
}

![avatar](/picture/1.png)

![avatar](/picture/2.png)

配置实名者时请查看购票须知中是否有相关要求，下面两张图分别表示没有、有实名需求的情况：

![avatar](/picture/3.png)

![avatar](/picture/4.png)

若是首次登录，根据终端输出的提示，依次点击登录、扫码登录，代码将自动保存cookie文件（cookie.pkl）。

使用前请将待抢票者的姓名、手机、地址设为默认，如存在多名实名者，请提前保存相关信息。

配置完成后执行python Autoticket.py即可，由于有启动延迟，建议提前一段时间打开程序。

## Advance usage
最后成功测试运行时间：2019-10-06。

此方法太过于依赖大麦网页面源码的元素的title、Xpath、class name、tag name等，若相应的绝对路径寻找不到则代码无法运行。

建议自己先测试一遍，自行修改相应的绝对路径或用更好的定位方法替代。

具体定位方案请参见Wiki。

本代码可修改为防弹窗类异常的持续抢票，仅需修改代码末尾：解注释"while True"与"break"，注释"if True"即可。

    # while True: # 可用于无限抢票，防止弹窗类异常使抢票终止
    if True:
        try:
            con.enter_concert()
            if con.type == 1:  # detail.damai.cn
                con.choose_ticket_1()
                con.check_order_1()
            elif con.type == 2:  # piao.damai.cn
                con.choose_ticket_2()
                con.check_order_2()
            con.finish()
            # break
        except Exception as e:
            print(e)

## Change log
v0.1: 

基本功能实现：

  1）用户登录cookie记录
  
  2）场次、票档自动勾选，优先级设定，自动跳过无票/缺货登记
  
  3）实名者/观演人设定
  
v0.2：

鲁棒性提升：

  1）添加用户昵称，验证登录成功
  
  2）修改提交订单按钮的索引方式，增强适配性
  
v0.3:

增强适配性，添加piao.damai.cn类别网页支持

v0.4:

鲁棒性提升，修改终端输出内容，添加指定购买票数功能（暂未支持勾选多实名者）

v0.5:

改默认浏览器为Chrome，默认取消图片加载，修复了部分bug，支持detail类别网站的票数增减、多实名者勾选，调整部分定位方式，修改错误输出

v0.6:

增加日期选择功能，完善实名售票，增强异常处理与第2类网址适配
  
## To-do List

1. 完善第2类网址（piao.damai.cn）实名购票功能

2. 鲁棒性增强

3. 适配手机APP端（路漫漫~）

## Ref
本代码修改自Ref 1，2两个Repo。

1. Oliver0047: https://github.com/Oliver0047/Concert_Ticket

2. MakiNaruto: https://github.com/MakiNaruto/Automatic_ticket_purchase

3. JnuMxin: https://github.com/JnuMxin/damaiAuto
