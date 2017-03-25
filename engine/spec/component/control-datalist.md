# SPC10001：配置子表单中的DataList表格

## 1. Example

## ![](/engine/spec/component/img/op-003-01.png)2. Tree Map

## ![](/engine/spec/component/img/op-003-02.JPG)3. Comments

* 控件配置

* ```
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
    * data：配置Redux路径，告诉当前DataList





