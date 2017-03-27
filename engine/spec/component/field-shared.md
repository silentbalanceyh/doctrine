# UCA20001：共享配置说明

## 1. Tree Map

![](/engine/spec/component/img/field-005-01.JPG)

## 2. Attributes

### 2.0. 顶层配置



### 2.1. 提示类

* **hint**：水印提示效果，目前版本中直接使用特殊配置——asterisk = "required"时该值为"（必填）"
* **tips**：浮游提示效果，点击选中的时候使用的提示信息

### 2.2. 必填可选

* **asterisk：**支持两个值——required/optional

### 2.3. 图标/单位

* **icon**：图标效果，可设置当前输入框对应的图标信息
* **unit**：计量单位信息，可设置当前输入框对应的计量单位
* **position**：图标/计量单位共享配置，用于指定图标以及计量单位的左右位置，支持两个值——left/right

### 2.4. 布局信息

* **inline**：该值仅仅对非FieldSet的布局生效，表示图标和输入框同行配置
* **fieldset**：FieldSet类型的Form专用配置信息，指定FieldSet每一个部分的名字
* **location**：FieldSet类型的Form专用位置信息的配置

### 2.5. 状态信息

* **readonly**：当前字段为只读

### 2.6. 风格信息

* **style**：当前字段对应的风格专用信息

## 3. Form State

* **ajouter**：添加状态的Form
* **modifier**：编辑状态的Form
* **commun**：通用状态的Form，一般用于搜索条件
* **vue**：查看状态的Form，一般用于查看信息



