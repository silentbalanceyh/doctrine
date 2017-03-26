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
5. 当前组件专用配置

   ```
   "ajouter":{
       ...
       "monitor":{
           "attriveTime":["form","fmRoomForm","values","attriveTime"],
           "leaveTime":["form","fmRoomRoom","values","leaveTime"]
       },
       "duration":{
           "from":"arriveTime",
           "to":"leaveTime"
       },
       "limitation":{
           "attriveTime":"days",
           "leaveTime":"days"
       },
       "pattern":"YYYY-MM-DD HH:mm",
       "mode":"days"
   }
   ```

   1. monitor：计算duration所需要的时间字段信息
   2. limitation：计算时间字段时变更的限制条件，如果是days则表示仅仅是天发生改变的时候触发重算，值根据Moment.js中支持的值来处理
   3. duration：专用配置，仅包含from和to两个键值，from表示开始，to表示结束
   4. pattern：解析时间格式用的pattern
   5. mode：计算时用的维度，时间计算维度包括：年、月、日、时、分、秒



