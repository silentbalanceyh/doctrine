# SPC10001：配置子表单中的DataList表格

## 1. Example

## ![](/engine/spec/component/img/op-003-01.png)2. Tree Map

## ![](/engine/spec/component/img/op-003-02.JPG)3. Comments

1. 控件配置

2. ```
   # ui.control
   ...
   "spec":{
       ...
       "table":{
           "data":{
               "roomlist":["form","fmForm","values","orderItems"]
           },
           "roomlist":[
               {
                   ...
               }
               ...
           ]
       }
   }
   ```

   * Required：必须配置的内容：
     * data：配置Redux路径，告诉当前DataList数据从哪个路径中读取
     * &lt;key&gt;：配置当前DataList中的列信息，类似List控件中的columns中的列配置
3. mapping中的配置
   mapping中的配置根据控件的使用有所差异，默认情况下，目前的控件使用了tables用于处理，最终映射变量名为`$_table。`

## 4. Development

**Code**

```
File: 
rtv/src/spirit/hotel/OrderContainer/UIAssist.js
rtv/src/spirit/hotel/OrderContainer/DataList.js
```



