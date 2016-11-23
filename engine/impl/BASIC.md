# 实施备注

## 1.启动顺序

1. ZKBoujour
2. RepertoryBoujour
3. MetaBoujour
4. DataBoujour
5. EngineBoujour
6. DeployBoujour: Meta -> Data

## 2.MetaBoujour启动参数

	final boolean schema = true;

* schema为true：需要重新Deploy表结构，表结构发生改变的时候运行
* schema为false：数据库表结构不改变；

## 3.数据目录

* htl：没有schema
* oob：没有data

### 3.1.Schema

Meta

* category: `ENTITY | RELATION`
* mapping: `DIRECT`
* policy: `COLLECTION | INCREMENT | GUID | ASSIGNED`

Keys

* name: `FK_ | UK_ | PK_`，所有的name不重复，`PREFIX + <TABLE> + <Field>`
* category: `ForeignKey | UniqueKey | PrimaryKey`
* multi: 是否多字段
* columns：根据multi进行字段设置

Fields

* 基本：`name, type, columnName, columnType`：columnType（数据库映射文件）<br/>
映射文件路径：`engine/database/mapping/<Database>/mapping.properties`
* String类型：`length` -> STRING1（小文本），STRING2（大文本）
* Date类型
	* datetime：`STRING | TIMER`；
	* dateformat：格式
* Decimal类型：
	* length：宽度设置
	* precision：精度设置
* FK类型：
	* foreignkey = true
	* refTable：关联表
	* refId：关联字段
* UK类型：
	* unique = true
* PK类型：
	* primarykey = true
* 是否为空：
	* nullable = false（不可为空）

### 3.2.View写法

1. view：数据库视图名称`V_`；
2. statement
	1. 单纯的`JOIN`：可以加括号，也可以不加
	2. 复杂的`JOIN`：必须加括号
3. columns：columnName
	1. 直接写；
	2. 带前缀，启用别名；
	3. 支持重命名；

### 3.3.VertX

#### 3.3.1.address

`address`：三个字段无任何重复

基本规范：

* `workClass`：Verticle（Worker类型）;
* `consumerAddr`：消息地址：`MSG://MESSAGE/QUEUE/*`开头；
* `consumerHandler`：Consumer组件；

目前配置：

* `DATA`：访问SQL数据库用的消息总线；
* `PAGER`：分页查询；
* `VIEW`：所有访问ViewRecord；
* `BASIC/OAUTH`：各自分Channel；
* `SNAPS`：快照功能；

### 3.3.2.verticle

* worker：是否Worker类型

### 3.3.3.Restful

**Route**:

`oob/vertx/route/`

Required：`path`

* `path`：请求路径，最终路径：`parent` + `path`

Optional：

* `parent`：`/api`
* `consumerMimes`：`json`：Content-Type
* `producerMimes`：`json`：Accept
* `requestHandler`：`com.viee.impl.request.EngineExecutor`
* `failureHandler`：NULL
* `method`：`GET`
* `order`：500000

`order`配置；

`engine/handler/orders.properties`

* 10000：跨域访问：Cors
* 40000：Cookie访问
* 70000：Body访问
* 100000:Uri配置：`/api/:name`，默认自动添加——自己开发
* 200000:RequestAcceptor：404，405，406，415——自己开发
* 250000:SecureKeaper：安全认证——自己开发
* 300000:Session
* 350000:User Session
* ------------------------------------------
* 400000:（保留）模板引擎：Template Engine
* 420000:（保留）静态资源：Static
* 440000:（保留）Favicon的Handler
* ------------------------------------------
* 400000:（保留）动态引擎（带安全）
* 440000:（保留）动态引擎
* ------------------------------------------
* 400000: RequestSinker：请求读取
* 420000: DataInspector：数据验证
* 430000: DataStrainer：数据转换
* 440000: RequestStdner：标准化流程
* 450000: SignKeaper：支持签名
* 500000: EngineExecutor：（可配置）
* 1000000: Failure/Service

**Uri/Rules/Script**：

Uri路径计算：

`oob/vertx/uri/*`

目录命名：

* 地址 + 方法：地址处理Pattern；
* Route: /oth/login/, Method: POST<br/>URI: api.oth.login/post/

数据文件：

1. URI本身数据：目录 + `data.json`；
2. Script数据：目录 + `script.json`；
3. 脚本文件：Script Name + `.js`；
4. 规则文件：目录 + `rules`：`v-`：验证器，`c-`：转换器

**1.1.Uri**

`data.json`

Required：

* uri：必填，全路径；
* identifier：必填，全局ID；
* script：（PEScript的name字段，JS文件名）；
* requiredParam：必须的参数列表；
* responder：响应格式；

Optional：

* address：消息地址，默认：`MSG://MESSAGE/QUEUE/DATA`；
* method：默认GET；
* paramType：`QUERY | BODY | FORM | CUSTOM`，默认`QUERY`；
* returnFilters：返回字段过滤器，对应Schema中Field Name；
* contentMimes/acceptMimes：默认都是application/json；
* sender：定制消息发送者；

**1.2.Rules**

* name：对应参数名；
* type：自定义类型，自己定义的10种；
* order：执行顺序；
* componentType：`VALIDATOR | CONVERTOR`（验证/转换）
* componentClass：Java类名；
* config：（随便写）
* errorMessage：显示信息

**1.3.Script**

* name：文件名
* namespace：名空间
* content：配置了直接读，未配置：当前目录下的`<name>.js`

### 3.3.4.Script

**Cond**

查询条件专用引擎，用于生成Expression的Java条件类；

* `Cond.EQ`：等于；
* `Cond.NEQ`：不等于；
* `Cond.LT`：小于；
* `Cond.LE`：小于等于；
* `Cond.GT`：大于；
* `Cond.GE`：大于等于；
* `Cond.NULL`：为空；
* `Cond.NOTNULL`：不为空；
* `Cond.LIKE`：匹配，对应SQL的LIKE；
* `Cond.OR`：对应SQL复杂OR；
* `Cond.AND`：对应SQL复杂AND；

**MatchMode**

仅仅用于LIKE关键字：

* `MatchMode.END`：后置匹配；
* `MatchMode.EXACT`：相当于EQ；
* `MatchMode.START`：前置匹配；
* `MatchMode.ANYWHERE`：任意匹配；

**Type**

主要用于连接类型数据，传入值

* `Type.String`：字符串
* `Type.Json`：Json数据
* `Type.Xml`：Xml数据
* `Type.Script`：脚本
* `Type.Boolean`：Boolean类型
* `Type.Long`：长整形
* `Type.Integer`：整数类型
* `Type.Binary`：字节类型
* `Type.Date`：时间格式类型
* `Type.Decimal`：带精度数据类型

**Value**

主要用于值处理

* `Value.Get`：读取值；
* `Value.Updated`：更新值读取；

**Dao**

主要用于数据库访问

* `Dao.Remove`：删除；
* `Dao.Insert`：插入；
* `Dao.Update`：更新；
* `Dao.BatchRemove`：批量删除；
* `Dao.BatchInsert`：批量插入；
* `Dao.BatchUpdate`：批量更新；
* `Dao.Modify`：按照条件更新；
* `Dao.Unique`：读取唯一记录；
* `Dao.Query`：按照条件读取记录集；
* `Dao.Collect`：读取多记录；

**核心方法**

* `__DataWriter`：写入器（综合）
* `__QueryEngine`：读取器（综合）
* `__Lang`：处理国际化

**环境对象**

* $env：环境对象；
* $data：请求参数；

### 3.3.5.Component

自定义组件：`com.vie.un`

**com.vie.un.exchange**

* Replier：Address -> consumerHandler
* Sender：Uri -> sender

**com.vie.un.flow**

* Repdor：Uri -> responder

**com.vie.un.uca**

* Validator:
* Convertor: Rule -> componentClass

**com.vie.un.vtc**

* Worker，Publisher
* Standard（Agent）

**com.viee.impl.request**

RequestHandler

### 3.3.6.Restart Required

* Schema更改：Meta -> Data -> Engine
* Restful更改：Meta -> Engine
* Data更改：Data

### 3.3.基本命名

1. 名空间：
	1. Schema：`com.vie.****`
	2. View：`com.viev.****`
2. identifier：
	1. DataRecord：不能以`v.`和`meta-`开头；
	2. ViewRecord：`v.`开头；
	3. MetaRecord：`meta-`开头，不存储在H2中：``
1. identifier
	1. abs：抽象层Abstract
	2. atm：财务系统
	3. ecm：在线购物
	4. hr：人力资源
	5. htl：（目前的系统Hotel）
	6. oth：OAuth认证
	7. psi：进销存系统
	8. res：基本资源表
	9. sec：RBAC安全模型
	10. vip：会员系统
	11. vie：系统引擎
	12. ui：界面配置
	13. 所有关联表在二级命名：xxx.rel.
	14. 所有记录表也在二级命名：xxx.his.
	15. 进销存不同订单：xxx.ord.
2. 字段名称：
	1. PK_ID：主键GUID只能StringType，INCREMENT只能LongType, IntType
	2. 主要字段：`S_`
	3. 最后字段：`Z_ -> Z_LANGUAGE, Z_CREATE_BY, Z_CREATE_TIME, Z_UPDATE_BY, Z_UPDATE_TIME`
	4. 树搜索字段：`I_LEFT, I_RIGHT, I_LEVEL`
	5. 所有FK关联字段：`R_**_ID`
	6. 软关联：`R_**`
	7. 固定值：`RLT_` -> `SYS_LIST_FIXED`
	8. 类型字段：`H_CAT_ID`
	9. 界面化：`UI_`
	10. Boolean值：`IS_`
	11. 脚本字段：`J_`
	12. 元数据字段：`RM_`数据来自H2
	13. 用户信息字段：`U_`
		1. `U_OPERATOR`：操作人
		2. `U_HANDLER`：经办人
		3. `U_MANAGER`：负责人
		4. `U_AGENT`：前台
		5. `U_GODOWN`：仓库管理员
	14. 签名相关的：`E_`

