# 办理入住

[Database Design](db/DBDESC001.xlsx)

## 1.特殊流程说明

* 如果存在排房记录，则入住房间信息直接从排房记录中提取，排房记录使用下拉方式选择；
* 房间类型、房间号码为特殊控件，直接通过计算得出，计算依据为目前的订单项中已经预定的房型和房间；
* 裸入住则直接以房间号作为基础，不包含任何预定订单，仅修改房间当天的状态信息；
* 宾客信息从宾客档案中提取，如果是新入住宾客则直接添加新的宾客档案；
* 宾客入住次数按照上边宾客信息进行运算；
* 备案人员关联ID则以上传备案资料的操作员为主；

## 2.特殊值说明

**`HTL_OCCUP`**

* `RLT_OCCUP_TYPE`：入住类型：inoccup.type
	* Single：散客入住
	* Team：团队入住
* `RLT_OCCUP_STATUS`：入住状态：inoccup.status
	* Created：新创建
	* Registered：已登记
	* Delay：延迟
	* Cancel：撤销

**`HTL_TRAVELER`**

* `RLT_TRAVELER_STATUS`：宾客状态：traveler.status
	* OnGoing：在住宾客
	* History：历史宾客
* `S_IDC_TYPE`：证件类型：credential.type
	* First：一代身份证
	* Second：二代身份证
	* Driving.License：驾驶证
	* Passport：护照
	* Osifer：军官证
	* Soldier：士兵证
	* HK.Macau：港澳通行证
	* Home.Return：回乡证
	* Interim：临时身份证
	* Household：户口薄
	* Police：警官证
	* Other：其他

## 3.特殊控件交互说明

* hotel.DropSchedule：下拉，可为空，如果已经存在排房记录，则直接从排房记录中读取，并填充房间类型、房间号
* hotel.DropRoomType：下拉，不存在排房记录时，从订单项中读取已经预定的房型信息，读取房型，入住完成（人数满了）的房型不显示
* hotel.FieldRoomNumber：二级下拉，选中房型过后，存在排房记录从排房记录中预定的房间读取，如果不存在排房记录从系统计算的满足条件的第一个房间中提取



