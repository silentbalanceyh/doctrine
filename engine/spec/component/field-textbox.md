# UCA20001：field.TextBox

## 1. Tree Map

![](/engine/spec/component/img/field-001-01.JPG)

## 2. Comments

1. 共享专用配置（所有的Field都有的配置）
   1. type：必须为field.TextBox
   2. cid：在当前Page唯一，如果有多个Form的时候也必须唯一
   3. name：字段名称，在当前Form中必须唯一（推荐在Page唯一）
2. FieldSet专用（在FieldSet Form中inline失效）
   1. 1. fieldset：面板名称
      2. location：当前字段所在位置
   2. 
3. 提示信息专用
   1. label：左边显示的标签文本
   2. hint：提示信息（水印提示效果）
   3. tips：浮游提示效果
   4. asterisk：（required \| optional ）提示信息（本来是前置的必填输入，转换成hint=（必填））

1. Icon图标位置
   1. icon：图标类型
   2. position：（left \| right）图标位置
2. 字段布局配置
   1. inline：（FieldSet中只能为inline，横向排列字段信息）



