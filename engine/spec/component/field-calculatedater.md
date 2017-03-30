# UCA20010：field.CalculateDater

## 1. Tree Map

![](/engine/spec/component/img/field-010-01.JPG)

## 2. Attributes

共享配置说明参考：[UCA20001：共享配置说明](/engine/spec/component/field-shared.md)

* 顶层：cid, name, type = **"field.CalculateDater"**
* 提示类：label, hint, tips
* 必填/可选：asterisk = \( required/optional \)
* 图标/单位：icon, unit, position = \( left/right \)
* 布局信息：inline, fieldset, location

## 3. Special

```
"ajouter":{
    ...
    "format":{
        "date":"YYYY-MM-DD",
        "time":"HH:mm"
    },
    "monitor":{
        "arriveTime":["form","fmRoomForm","values","arriveTime"],
        "insideDays":["form","fmRoomForm","values","insideDays"]
    },
    "calculate":{
        "start":"arriveTime",
        "duration":"insideDays"
    },
    "mode":"days"
}
```

* **format**：当前时间控件专用，设置日期部分和事件部分格式
* **monitor：**计算目标时间所需要的字段信息
* **calculate：**计算因子，使用开始时间字段和结束事件字段计算当前事件
* **mode**：决定了duration的模式，单位信息：年、月、日、时、分、秒、毫秒



