# 服务端接口定义

> 版本：v1.5
> 作者：李猛

## 修订记录

1. v1.0, 2017-05-15, limeng, 初始版本。
2. v1.1, 2017-05-18, zangwei, 修改上传文件接口、派工接口和消息推送接口。
3. v1.2, 2017-05-22, zangwei, 修改下载工单数据接口、获取派工列表接口、上传多个文件接口和消息定义-新工单接口。
4. v1.3, 2017-05-03, limeng, 修改工单下载接口和工单处理接口。
5. v1.4, 2017-09-06, limeng, 新增管理人员获取手下施工人员接口、新增、任务查询接口，重新定义一下获取用户信息接口的roles，用来判断是不是管理员。
6. v1.5, 2017-09-13, limeng, 非居施工的施工人员字段类型由number改为string


## 概述
本文主要定义任务移动端接口定义规范。



## 激活设备接口

激活设备接口

* URL: `v1/mobile/devices/register`
* METHOD: `POST`

* 请求内容:

    ```
    {
      macAddress: 'mac地址', // string
      deviceID: '设备号', // string
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        isActived : true,         // boolean, true：激活成功，false：激活失败
        error : '设备未入库',      // string, 激活失败返回信息，否则为''
        activeTime : 123456,      // number, 激活时间, utc
        activeCode : 'abc123',    // string, 激活码, 123456
        activeState :　'正常',     // string, 激活状态, 正常
        deviceState :　'正常'      // string, 设备状态, 正常
      }
   }
   ```


## 登录接口

登录

* URL: `v1/mobile/auth`
* METHOD: `POST`

* 请求内容:

    ```
    {
      account: '登录账号', // string
      pwd: '密码', // string
      appIdentify: 'sh3h', // string 
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        "accessToken": "sample string 1", // string
        "userId": 2 // number
      }
   }
   ```


## 获取用户信息接口

获取用户信息接口

* URL: `v1/mobile/user/{userId}`
* METHOD: `GET`

* 参数:
    * userId: 用户Id

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
         "userId": 1, // number
         "userName": "sample string 2", // string
         "sex": 1, // number
         "account": "sample string 3", // string
         "address": "sample string 4", // string
         "mobile": "sample string 5", // string
         "tel": "sample string 6", // string
         "roles": "1, 2, 3, ...", // string （规定10000角色是管理人员）
      }
   }
   ```


## 管理人员获取施工人员接口

获取获取施工人员接口

* URL: `v1/mobile/constructors/{userId}`
* METHOD: `GET`

* 参数:
    * userId: 管理人员Id

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
         "userId": 1, // number
         "userName": "sample string 2"
      }
   }
   ```


## 位置追踪

上传位置信息

* URL：`v1/mobile/track`
* METHOD：`POST`

* 请求内容：
  
  ```
  {
    userId: 123, // number
    deviceId: 'abc', // string, 设备Id
    location: {      // 位置信息
      locationType： 'wgs84', // string, 坐标类型
      longitude： 121.3, // number, 经度
      latitude: 31.5, // number, 纬度
      radius: 0.0, // number, 精度
      altitude: 0.0, // number, 高度
      direction: 0.0, // number, 方向
      speed: 0.0, // number, 速度
      time: 1431619200000 // number, 采集时间, utc
      extendInfo: '扩展信息' // string
    }
  }
  ```

* 回复内容：

    ```
    {
        code: 0,
        statusCode: 200,
        message: '操作描述',
        data： {
          time: 1431619200000 // number, 服务器当前时间, utc
        }
    }
    ```


## APP更新

检查app是否可以更新

* URL: `v1/mobile/update/app/check?version={version}`
* METHOD: `GET`

* 参数:
   * version: 1, // number, 版本号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        version: 2, // number
        url：'http://example.com/publish/app/app_id_app_v12.zip' //string
      }
    }
    ```


## 数据包更新

检查数据包是否可以更新

* URL: `v1/mobile/update/data/check?version={version}`
* METHOD: `GET`

* 参数:
   * version: 1, // number, 版本号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        version: 2, // number
        url：'http://example.com/publish/app/app_id_app_v12.zip' //string
      }
    }
    ```


## 下载工单数据

根据条件获取工单数据

* URL: `v1/mobile/works/list?userId={userId}&type={type}&district={district}&date={date}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * type: '业务类型', // number
   * district: '小区名称', // string
   * date: '时间' // number, utc

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
          {
            projectNo: '工程编号', // string
			businessType: '业务类型', // number, 1.踏勘销单;2.新装销单;3.拆除销单;4.移添改销单;5.迁装销单;6.换表销单;7.工程验收销单;8.维修销单;9.抢修销单;10.非居施工验收销单
			projectType: '工程类型', // number, 1.扩缩表;2.移装;3.排管；4.新装工程
			workCount: '踏勘次数/施工次数/验收次数', // number
			acceptTime: '受理时间', // number, utc
			userNo: '用户编号', // string
			userName: '用户名称', // string
			userAddress: '用户地址/用气地址', // string
			oldUserAddress: '旧用户地址', // string
			appointDate: '预约日期', // number, utc
			meterBarCode: '表条形码', // string(多个表中间以@来串联)
			meterEnergy: '表能量', // string(多个表中间以@来串联)
			meterPosition: '表位', // string(多个表中间以@来串联)
			delegateToConstruct: '委托表后施工', // string
			switchMeterNo： '换表编号', // string
			applyDate: '申请日期', // number, utc
			dispatchDate: '派工日期', // number, utc
			switchMeterReason: '换表原因', // string
			oldMeterNo: '旧表表号/表号', // string
			oldMeterManufacturer: '旧表厂商', // string
			oldMeterType: '旧表型号', // string
			oldMeterReading: '旧表抄码/上次抄见数', // number(多个表中间以@来串联)
			checkType: '验收类型', // string
			maintainType: '维修类型', // string, 维修、报修、抢修
			projectState: '工程状态', // string
			acceptStation: '受理站点', // string
			useGasProperty: '用气性质', // string
			breakdownDescription: '故障描述', // string
			reportCondition: '用户反映情况', // string
			extendInfo: 'json string' // string, 扩展信息, 可为空
          },
          ......
      ] 
    }
    ```
	

## 修改预约时间

修改预约时间

* URL: `v1/mobile/work/appointDate/modify?userId={userId}&projectNo={projectNo}&isAm={isAm}`
* METHOD: `POST`
* 参数:

   * userId: '用户id', // string
   * projectNo: '工程编号', // string
   * isAm: true, // bool, true: 上午, false: 下午
   

* 返回内容：

    ```
    {
      code: 0, // 0: 预约成功;  其它值: 预约失败
      statusCode: 200,
      message: '操作描述', // 空: 预约成功, 非空: 失败原因
      data: null
    }
    ```


## 处理开阀

处理开阀

* URL: `v1/mobile/work/valveState/modify?userId={userId}&projectNo={projectNo}&isOpen={isOpen}`
* METHOD: `POST`
* 参数:

    * userId: '用户id', // number
    * projectNo: '工程编号', // string
    * isOpen: true, // bool, true: 开, false: 关

* 返回内容：

    ```
    {
      code: 0, // 0: 成功; 其它值: 失败
      statusCode: 200,
      message: '操作描述', // 空: 成功, 非空: 失败原因
      data: null
    }
    ```


## 工单处理

处理多条工单数据

* URL: `v1/mobile/works/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id
   
* 请求内容:

    ```
    {
      [
        {
          projectNo: '工程编号', // string
		  processTime: '踏勘时间/验收时间/维修时间/竣工时间', // number, utc
		  processPerson: '踏勘人员/施工人员/验收人员/维修人员', // number
		  processPerson2: '非居施工人员姓名', // string
		  processResult: '踏勘结果/施工结果/验收结果/处理结果', // number
		  constructInfo: '施工信息/验收信息(多个施工信息中间以@来串联)', // string
		  lockedDoorNo: '门锁编号', // string
		  meterPosition: '表位', // number
		  oldMeterBarCode: '旧表表条形码', // string
		  newMeterBarCode: '表条码/新表表条形码', // string
		  gasOnDirection: '进气方向', // number
		  oldMeterReading: '拆表见表数/旧表抄见数/见表指数', // number
		  newMeterOriginReading: '新表底码', // number
		  oldMeterEnergy: '旧表表能量', // number
		  newMeterEnergy: '新表表能量', // number
		  newMeterNo: '新表表号', // string
		  breakdownReason: '故障原因', // string
		  occurReason: '发生原因', // string
		  resolveMethod: '解决措施',   // string
		  maintainMethod: '维修方法', // string
		  manMadeDamage: '人为破坏',//number, 0:否；1.是
		  remark: '备注', // string
		  extendInfo: 'json string', // string, 扩展信息, 可为空

		  amount: '总金额', // number
		  materials: [{ // 材料, 没有则为null
			  type: '材料类型', // number
			  spec: '材料规格', // number
			  count: '数量', // number
			  unit: '单位', //number
			  name: '材料名称', // string
			  manufacturer:'材料厂商' // string
			},
			'''
		  ],

		  devices: [{ // 热水器及灶具信息, 没有则为null
			  kind: '设备类型', // number, 1: 热水器, 2: 灶具
			  brand: '品牌', // number
			  type: '类型', // number
			  spec: '规格', // number
			},
			...
		  ]
		},
		...
	  ]
	}
    ```

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
        {
          isSuccess: 'true',
          message: '',
          projectNo: '工程编号', // number
          extendInfo: 'json string' // 扩展信息, 可为空
        },
        {
          isSuccess: 'false',
          message: '失败原因',
          projectNo: '工程编号', // number
          extendInfo: 'json string' // 扩展信息, 可为空
        },
        ...
      }
    }
    ```


## 查询工单数据

根据条件查询工单数据

* URL: `v1/mobile/works/search?userId={userId}&type={type}&start={start}&end={end}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * type: '业务类型', // number
   * start: '开始时间', // number, utc
   * end: '结束时间' // number, utc

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
          {
            task:{
				projectNo: '工程编号', // string
				businessType: '业务类型', // number, 1.踏勘销单;2.新装销单;3.拆除销单;4.移添改销单;5.迁装销单;6.换表销单;7.工程验收销单;8.维修销单;9.抢修销单;10.非居施工验收销单
				projectType: '工程类型', // number, 1.扩缩表;2.移装;3.排管；4.新装工程
				workCount: '踏勘次数/施工次数/验收次数', // number
				acceptTime: '受理时间', // number, utc
				userNo: '用户编号', // string
				userName: '用户名称', // string
				userAddress: '用户地址/用气地址', // string
				oldUserAddress: '旧用户地址', // string
				appointDate: '预约日期', // number, utc
				meterBarCode: '表条形码', // string(多个表中间以@来串联)
				meterEnergy: '表能量', // string(多个表中间以@来串联)
				meterPosition: '表位', // string(多个表中间以@来串联)
				delegateToConstruct: '委托表后施工', // string
				switchMeterNo： '换表编号', // string
				applyDate: '申请日期', // number, utc
				dispatchDate: '派工日期', // number, utc
				switchMeterReason: '换表原因', // string
				oldMeterNo: '旧表表号/表号', // string
				oldMeterManufacturer: '旧表厂商', // string
				oldMeterType: '旧表型号', // string
				oldMeterReading: '旧表抄码/上次抄见数', // number(多个表中间以@来串联)
				checkType: '验收类型', // string
				maintainType: '维修类型', // string, 维修、报修、抢修
				projectState: '工程状态', // string
				acceptStation: '受理站点', // string
				useGasProperty: '用气性质', // string
				breakdownDescription: '故障描述', // string
				reportCondition: '用户反映情况', // string
				extendInfo: 'json string' // string, 扩展信息, 可为空
			},
			reply:{
				projectNo: '工程编号', // string
				processTime: '踏勘时间/验收时间/维修时间/竣工时间', // number, utc
				processPerson: '踏勘人员/施工人员/验收人员/维修人员', // number
				processPerson2: '非居施工人员姓名', // string
				processResult: '踏勘结果/施工结果/验收结果/处理结果', // number
				constructInfo: '施工信息/验收信息(多个施工信息中间以@来串联)', // string
				lockedDoorNo: '门锁编号', // string
				meterPosition: '表位', // number
				oldMeterBarCode: '旧表表条形码', // string
				newMeterBarCode: '表条码/新表表条形码', // string
				gasOnDirection: '进气方向', // number
				oldMeterReading: '拆表见表数/旧表抄见数/见表指数', // number
				newMeterOriginReading: '新表底码', // number
				oldMeterEnergy: '旧表表能量', // number
				newMeterEnergy: '新表表能量', // number
				newMeterNo: '新表表号', // string
				breakdownReason: '故障原因', // string
				occurReason: '发生原因', // string
				resolveMethod: '解决措施',   // string
				maintainMethod: '维修方法', // string
				manMadeDamage: '人为破坏',//number, 0:否；1.是
				remark: '备注', // string
				extendInfo: 'json string', // string, 扩展信息, 可为空

				amount: '总金额', // number
				materials: [
					{ // 材料, 没有则为null
					  type: '材料类型', // number
					  spec: '材料规格', // number
					  count: '数量', // number
					  unit: '单位', //number
					  name: '材料名称', // string
					  manufacturer:'材料厂商' // string
					},
					'''
				],

				devices: [
					{ // 热水器及灶具信息, 没有则为null
					  kind: '设备类型', // number, 1: 热水器, 2: 灶具
					  brand: '品牌', // number
					  type: '类型', // number
					  spec: '规格', // number
					},
					...
				]
			},
			files:[
				{
					fileName:'文件名',//string
					type: '照片类型', // number, 1: 现场环境, 2: 燃气表, 3: 燃气设备, 4: 装表位置, 4: 其它
					url:'图片服务器地址'//string
				},
				...
			]
          },
          ......
      ] 
    }
    ```


## 获取派工列表

根据条件获取派工列表

* URL: `v1/mobile/works/assign/list?userId={userId}&date={date}&businessType={businessType}&projectType={projectType}&isAssign={isAssign}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * date: '派工日期', // number, utc
   * businessType: '业务类型', // number
   * projectType: '工程类型', // number
   * isAssign: false, // boolean, true: 已派工; false: 未派工

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
          {
            projectNo: '工程编号', // string
			businessType: '业务类型', // number, 1.踏勘销单;2.新装销单;3.拆除销单;4.移添改销单;5.迁装销单;6.换表销单;7.工程验收销单;8.维修销单;9.抢修销单;10.非居施工验收销单
			projectType: '工程类型', // number, 1.扩缩表;2.移装;3.排管；4.新装工程
			workCount: '踏勘次数/施工次数/验收次数', // number
			acceptTime: '受理时间', // number, utc
			userNo: '用户编号', // string
			userName: '用户名称', // string
			userAddress: '用户地址/用气地址', // string
			oldUserAddress: '旧用户地址', // string
			appointDate: '预约日期', // number, utc
			meterBarCode: '表条形码', // string(多个表中间以@来串联)
			meterEnergy: '表能量', // string(多个表中间以@来串联)
			meterPosition: '表位', // string(多个表中间以@来串联)
			delegateToConstruct: '委托表后施工', // string
			switchMeterNo： '换表编号', // string
			applyDate: '申请日期', // number, utc
			dispatchDate: '派工日期', // number, utc
			switchMeterReason: '换表原因', // string
			oldMeterNo: '旧表表号/表号', // string
			oldMeterManufacturer: '旧表厂商', // string
			oldMeterType: '旧表型号', // string
			oldMeterReading: '旧表抄码/上次抄见数', // number(多个表中间以@来串联)
			checkType: '验收类型', // string
			maintainType: '维修类型', // string, 维修、报修、抢修
			projectState: '工程状态', // string
			acceptStation: '受理站点', // string
			useGasProperty: '用气性质', // string
			breakdownDescription: '故障描述', // string
			reportCondition: '用户反映情况', // string
			extendInfo: 'json string' // string, 扩展信息, 可为空         
          },
          ......
      ] 
    }
    ```
  

## 派工

派工

* URL: `v1/mobile/works/assign/{userId}`
* METHOD: `POST`
* 参数:
   * userId: '用户id', // number
   
* 请求内容:

    ```
    {
      constructorId: '施工人员id', // number
      projectsNo: [ // string array, 工程编号
        '123', '456', '789', .... 
      ]
    }
    
    ```

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
        {
          isSuccess: true, // boolean, true: 派单成功
          message: '', // string
          projectNo: '工程编号', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        },
        {
          isSuccess: false, // boolean, false: 派单失败
          message: '失败原因', // string
          projectNo: '工程编号', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        },
        ...
      ]
    }
    ```


## 下载安检数据

根据条件获取安检数据

* URL: `v1/mobile/securityCheck/list?userId={userId}&district={district}&date={date}`
* METHOD: `GET`

* 参数:
    * userId: '用户id', // number
    * district: '小区名称', // string
    * date: '时间' // number, utc

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
          { 
            securityCheckNo: '安检编号', // string
            securityCheckType: '安检类型', // number, 1: 安检回填, 2: 整改回填, 3: 安检回访回填
            userNo: '用户编号', // string
            userName: '用户名称', // string
            userAddress: '用气地址', // string
            meterBarCode: '表条形码', // string  
            securityCheckInfo: '安检信息', // string
            reformInfo: '整改信息', // string
            extendInfo: 'json string' // 扩展信息, 可为空	
          },
          ......
      ] 
    }
    ```


## 安检回填

处理多条安检数据

* URL: `v1/mobile/securityCheck/process?userId={userId}`
* METHOD: `POST`
* 参数:
   * userId: '用户id' // number
   
* 请求内容:

    ```
    {
     [
      {
        securityCheckNo: '安检编号', // string
        securityCheckContent: '安检内容', // string
        reformContent: '整改内容', // string
        dangerLevel: '隐患等级', // number
        securityCheckIndex: '安检指数', // string 
        processPerson: '安检人员、回访人员、整改人员', // number 
        processTime: '安检时间、整改时间、回访时间', // number UTC时间
        processType: '安检回填类型、回访回填类型', // number
        lockedDoorNo: '门锁编号', // string
        reformType: '整改类型', // number
        extendInfo: 'json string' // string, 扩展信息, 可为空	   
      },
      ...
     ]
    }
    ```

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
        {
          isSuccess: true, // boolean, 成功
          message: '', // string
          securityCheckNo: '安检编号', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        },
        {
          isSuccess: false, // boolean, 失败
          message: '失败原因', // string
          securityCheckNo: '安检编号', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        },
        ...
      }
    }
    ```


## 上传多个文件

上传指定文件

* URL: `v1/mobile/files/upload/{no}/{isProject}/{type}`
* METHOD: `POST`
* 参数：
   * no: '工程编号或安检编号', // number  
   * isProject: true, // bool, true: 工程编号, false: 安检编号
   * type: '照片类型', // number, 1: 现场环境, 2: 燃气表, 3: 燃气设备, 4: 装表位置, 4: 其它

* 请求内容：

  文件内容, byte数组，支持多个文件同时上传，同时上传文件名(包括后缀，如:.jpg, .png, .mp3, .aac)

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
        {
          isSuccess: true, // boolean, 上传成功
          message: '', // string
          originName: '原始文件名', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        },
        {
          isSuccess: false, // boolean, 上传失败
          message: '失败原因', // string
          originName: '原始文件名', // string
          extendInfo: 'json string' // string, 扩展信息, 可为空
        }
        ....
      ]
   }
   ```


## 获取词语信息

获取词语信息

* URL: `v1/mobile/words/list?group=all`
* METHOD: `GET`

* 参数：
   * group, 词语分组，如果传⼊all或者空则返回所有词语组信息

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
       {               
         id: '词语Id',
         parentId: '父级Id',
         group: '词语组',
         name: '词语文本',
         value: '词语值',
         valueEx: '词语值扩展内容',
         remark: '备注'
       },
       ...
     ]
   }
   ```



## 消息定义

消息内容为JSON字符串。具体内容根据消息类型不同而变化。

消息类型包含：

* 新工单, 201
* 新安检, 202
* 取消工单, 203
* 取消安检, 204

### 新工单

当新的工单被派遣给指定人后发生。一次消息内可能携带多个工单数据（视内容而定）。

* 工单数据较少时(比如：1~2条)，直接发送工单内容

    ```
    {
      type: '201', // number
      content: [ // 任务内容, array
        {
          projectNo: '工程编号', // string
		  businessType: '业务类型', // number, 1.踏勘销单;2.新装销单;3.拆除销单;4.移添改销单;5.迁装销单;6.换表销单;7.工程验收销单;8.维修销单;9.抢修销单;10.非居施工验收销单
		  projectType: '工程类型', // number, 1.扩缩表;2.移装;3.排管；4.新装工程
		  workCount: '踏勘次数/施工次数/验收次数', // number
		  acceptTime: '受理时间', // number, utc
		  userNo: '用户编号', // string
		  userName: '用户名称', // string
		  userAddress: '用户地址/用气地址', // string
		  oldUserAddress: '旧用户地址', // string
		  appointDate: '预约日期', // number, utc
		  meterBarCode: '表条形码', // string(多个表中间以@来串联)
		  meterEnergy: '表能量', // string(多个表中间以@来串联)
		  meterPosition: '表位', // string(多个表中间以@来串联)
		  delegateToConstruct: '委托表后施工', // string
		  switchMeterNo： '换表编号', // string
		  applyDate: '申请日期', // number, utc
		  dispatchDate: '派工日期', // number, utc
		  switchMeterReason: '换表原因', // string
		  oldMeterNo: '旧表表号/表号', // string
		  oldMeterManufacturer: '旧表厂商', // string
		  oldMeterType: '旧表型号', // string
		  oldMeterReading: '旧表抄码/上次抄见数', // number(多个表中间以@来串联)
		  checkType: '验收类型', // string
		  maintainType: '维修类型', // string, 维修、报修、抢修
		  projectState: '工程状态', // string
		  acceptStation: '受理站点', // string
		  useGasProperty: '用气性质', // string
		  breakdownDescription: '故障描述', // string
		  reportCondition: '用户反映情况', // string
		  extendInfo: 'json string' // string, 扩展信息, 可为空  
        },
        ...
        { ... },
        { ... }
      ]
    }
    ```

* 工单数据较多时，直接发送工程编号

    ```
    {
      type: '201', // number
      content: ['123', '456', '789', 'aaa', ...] // array
    }
    ```


### 新安检

当新的安检被派遣给指定人后发生。一次消息内可能携带多个安检数据（视内容而定）。

* 安检数据较少时(比如：1~2条)，直接发送安检内容

    ```
    {
      type: '202', // number
      content: [ // 任务内容, array
        {
          securityCheckNo: '安检编号', // string
          securityCheckType: '安检类型', // number, 1: 安检回填, 2: 整改回填, 3: 安检回访回填
          userNo: '用户编号', // string
          userName: '用户名称', // string
          userAddress: '用气地址', // string
          meterBarCode: '表条形码', // string  
          securityCheckInfo: '安检信息', // string
          reformInfo: '整改信息', // string
          extendInfo: 'json string' // 扩展信息, 可为空  
        }
        ...
        { ... },
        { ... }
      ]
    }
    ```

* 安检数据较多时，直接发送安检编号

    ```
    {
      type: '202', // number
      content: ['123', '456', '789', 'aaa', ...] // array
    }
    ```


### 取消工单或安检
取消工单或安检   

  ```
  {
    type: '203/204', // number
    content: ['123', '456', '789', 'aaa', ...] // array
  }
  ```

