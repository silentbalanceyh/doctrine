# UCA20003：field.DurationBox

## 1. Tree Map

![](/engine/spec/component/img/field-003-01.JPG)

## 2. Comments

1. 共享专用配置（所有的Field都有的配置）
   1. type：必须为field.RemoteCombo
   2. cid：在当前Page唯一，如果有多个Form的时候也必须唯一
   3. name：字段名称，在当前Form中必须唯一（推荐在Page唯一）
2. 提示信息专用
   1. unit：计量单位信息，unit和icon对应的配置是冲突的，但是可共享position属性
3. FieldSet专用（在FieldSet Form中inline失效）
   1. fieldset：面板名称
   2. location：当前字段所在位置
4. 风格专用
   1. style：可将该style信息应用于input组件，所有组件都通用
5. 


