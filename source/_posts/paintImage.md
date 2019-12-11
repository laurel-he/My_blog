# 样本收集系统
## 需求背景
（1）由于业务扩展，原有样本库样本数量不足，精度不高，且缺乏年龄、性别等样本。原有的错误标记不够详细，例如只标记了性别错误，不能同时记录正确的信息；且没有一个年龄标记，当年龄判定错误时按照标记人预估填写正确的值；       
（2）原有的错误标记会直接修改掉作图状态，直接导致实际已出图但出图记录数量减少的问题，不利于计费系统判定；    
（3）由于标记人员对图片错误的判断不一定正确，可能标记错误；    
## 需求目标
（1）需要提供一个方便用户使用的样本采集系统，可在标记年龄、性别错误时填写正确值，作为训练样本以提高识别率    
（2）标记图片错误不应该修改出图的状态，不能影响每日统计报表和计费系统出图数统计；    
（3）所有标记进入样本库前应该由专业人员审核才能进入样本库；    
## 需求实现
（1）新增标记年龄和性别等状态的功能，当点击标记错误时，切割图片，获取所有人脸框，并展示对应的性别、年龄，可以修改每个人脸框的性别/年龄；     
（2）新增一个字段维护图片标记状态；新增一组数据维护图片正确的数据；    
（3）所有标记的图片进入审核流程，审核通过存储到样本库，不通过记录标记人并给标记人发送消息，
## 效果图
（1）列表预览效果：有建议直接加在质量总览里，需要确认        
![列表预览效果](https://wiki.jiapinai.com/lib/exe/fetch.php?media=%E7%A0%94%E5%8F%91:workflow%E5%B7%A5%E7%A8%8B%E6%95%88%E7%8E%87:biaoji_01.png "列表预览效果")    
（2）点击选择效果：有建议先选择错误，最后统一标记，可以做成批量，需要确认    
 ![点击选择效果](https://wiki.jiapinai.com/lib/exe/fetch.php?media=%E7%A0%94%E5%8F%91:workflow%E5%B7%A5%E7%A8%8B%E6%95%88%E7%8E%87:02.png "点击选择效果")     
（3）选择年龄效果-点击图2的用户头像，可编辑某个用户的年龄或性别，效果图省略
## 时序图

---
<uml>
@startuml
actor 标记人员
actor 审核人员 #red
participant 标记系统
participant 审核系统
标记人员->标记系统: 标记错误
标记系统->审核系统: 提交审核
participant 样本库
审核人员->审核系统: 审核通过
审核系统->样本库: 保存数据
@enduml

</uml>
---
## 表结构
1 photo表：   
在其中的data mixedData（原结构为array）中添加如下数据并存储：    

| 字段  | 含义 | 类型 | 默认值 |
|:-----:|------|------|:------:|
| status | 错误状态 | int | -1(-1:error 1:done) |
| remark | 标记 | string | '' |
| beforeAge | 修改前的年龄 | int |  |
| afterAge | 修改后的年龄 | int |  |
| beforeGender | 修改前的性别 | int |  |
| afterGender | 修改后的性别 | int |  |
| updateTime | 更新时间 | int |  |
| user | 操作人 | int |  |
## 流程图

---
<uml>
@startuml
start
: 开始标记;
: 获取所有人物框;
if (是否修改人物框数据) then (是)
    : 提交审核;
else (否)
    : 开始标记;
endif
    : 查看审核列表;
if (是否是管理员) then (是)
    : 审核标记样本;
if (审核通过) then (是)
    : 存储到样本库;
else (否)
    : 移除审核;
endif
endif
@enduml
</uml>
---

## 接口
1 标记年龄、性别    

* 请求方式：POST

* 描述：标记年龄、性别

* 请求参数：

| 字段  | 含义 | 类型 | 默认值 |
|:-----:|------|------|:------:|
| beforeAge | 修改前的年龄 | int |  |
| afterAge | 修改后的年龄 | int |  |
| beforeGender | 修改前的性别 | int |  |
| afterGender | 修改后的性别 | int |  |
| clusterId | 场景id | int |  |
| id | 照片id | int |  |

2 审核并存储到样本库

* 请求方式：POST

* 描述： 审核样本

* 请求参数：

| 字段  | 含义 | 类型 | 默认值 |
|:-----:|------|------|:------:|
| ids | 批量审核id | array |  |

## 测试要点
（1）修改人物性别、年龄并存储成功    
（2）是否存储到七牛云