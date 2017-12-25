# 服务端接口定义

> 版本：v1.0
> 作者：zang


## 修订记录

1. v1.0, 2017-09-05, zang, 初始版本。
2. v1.1, 2017-11-14, 新增调表信息,改动工单下载接口,以及数据上传接口,获取用户信息接口。
3. v1.2, 2017-12-2,工单处理接口新加拆表时抄码,强检编号,获取新加燃气表接口,获取收费项目信息
4. v1.3, 2017-12-20,新增ic卡现场制卡、补气、转存接口.电话检修质检接口.抄表质检接口.非居巡检接口.
5. v1.4, 2017-12-25,新增施工队长查询派单，查询派表，派表，派单，查询门锁，门锁确认，查询退单，退单确认接口.

## 概述
本文主要定义任务移动端接口定义规范。


## 登录接口

登录

* URL: `v1/mobile/auth`
* METHOD: `POST`

* 请求内容:
    ```
    {
      account: '登录账号',
      pwd: '密码',
      appIdentify: 'com.sh3h.gastask',
	  deviceId: '354360070074927'
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        "accessToken": "sample string 1",
        "userId": 2
      }
   }
   ```

## 登出接口

登出

* URL: `v1/mobile/logout`
* METHOD: `POST`

* 请求内容:
    ```
    {
      userId: 123, // number, 用户Id
	  deviceId: '354360070074927' // string
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: true, // bool, true: 成功，false：失败
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
         "roles": "1, 2, 3, ...", // string
         "team":'所属施工队',//string
         "thirdParty":"第三方施工员",//number  0:否 1：是
      }
   }
   ```

## 检查用户token是否有效

检查用户token是否有效

* URL: `v1/mobile/user/token/check`
* METHOD: `POST`

* 请求内容:
    ```
    {
      userId: 123, // number, 用户Id
	  deviceId: '354360070074927', // string
      accessToken: 'token值' // string
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        "tokenState": 1, // 1: accessToken有效; 2: accessToken更新; 3: 退出系统 
        "accessToken": 'token值' // 有效的accessToken值
      }
   }
   ```


## 下载工单数据

根据条件获取工单数据

* URL: `v1/mobile/tasks/list?userId={userId}&type={type}
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * type: '业务类型', // number 3:电话维修,1:调表

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber ：'用户编号' , //string
          taskNo:'任务编号(调表编号)',   		//string
          taskState: '任务状态',   //number 10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType: '任务类型',   	//number 3:电话检修     1:调表
          taskSubType: '任务子类型',	 //number 1:漏气维修 2：一般  
          cardNumber: '表号(旧表表号)',   	//string
          areaName: '区名',   	 //string
          userLevel: '用户等级',	 //string
          userName: '户名(用户名称)', 	 //string
          address: '地址', 		 //string
          telephone: '电话',  	//string
          mobilePhone: '手机',  	//string
          reflectPeople: '反映人',  	//string
          responseCategory: '反映类别', 	//string
          responseContent: '反映内容', 	//string
          reflectionForm: '反映形式', 	//string
          reactionTime: '反映时间(申请日期)',  	// number, utc
          roadType: '道路类型',		//string
          gasNature: '用气性质',		//string
          infoSources: '信息来源',		//string
          urgency: '紧急程度',		//string
          acceptTime: '接受时间',		// number, utc
          meterEnergy: '表能量(旧表能量)',		// number
          meterIndex: '表指数',				// number
          appointementStart: '预约开始',		//string
          appointementLength: '预约时长',		//string
          appointementEnd: '预约结束',	 //string
          repeat: '重复',   		// number 0:不重复 1：重复 
          barCode: '表条码',		 //string
          correctorBarCode: '校正仪条码',		//string
          settlementTmp: '结算温度',		 // number
          settlementPressure: '结算压力', 		//string
          supplyPressure: '供气压力',		 //string
          pulseEquivalent: '脉冲当量', 		//string
          urgent: '加急', 			// number 0：不加急,1:加急  
          dispatchExtend: '派单备注', 	//string  
          hangUp: '是否挂起', 	 // number 0:不挂 , 1:挂起
          reflectRemarks: '反映备注', //string
          meterModel: '型号(旧表型号)',   //string
          meterManufactor: '厂家(旧表厂商)', 	 //string
          calibratorSupplier: '校正仪供应商',  	//string
          calibratorModel: '校正仪型号', 	 //string
          processLevel: '处理级别', 	 //string
          infoNumber: '信息单号(PDA编号)', 		 //string
          serialNumber: '流水号', 		 //string
          receptionPerson: '受理人',	//string
          userNumber:'用户编号',  //string
          X: '经度',     // number
          Y: '纬度',     // number
          dispatchTime :'派工时间',  //number utc时间
          dispatchPersion:'派工人',  //string
          arriveTime:'到场时间',//long utc
          office:'办事处',//string 
          officeTelphone:'办事处电话',//string
          processTime: '剩余处理时间',//number
          officeAddress :'办事处地址',//string  
          lastOperateTime:'最近一次调表日期',number utc ------调表新加字段
          replaceReason:'换表原因',//string 
          spareNo:'备用表号',//string
		  diffValue:'差值',//string
		  caliber: '口径',//string
		  meterRead:'旧表抄码',//number
          installTime:'安装日期',//number utc 
          hangUpValue:'倒挂气量',//number
          AlarmValue:'报警气量'//number 
         },
         ........
      ]
    }



## 工单处理

处理多条工单数据  包括接单、到场、处理

* URL: `v1/mobile/tasks/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
      [
        {
          taskNo :'任务编号',  //string
          taskState :'任务状态', //number  10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType :'任务类型',   	//number 3:电话检修  1:调表
          taskSubType :'任务子类型',	 //number 1:漏气维修 2：一般  
          handleResult :'处理结果(施工结果)',  //string
          handleRemark :'处理备注(退单备注)', //string
          handleProcess :'处理经过', //string
          liveSituation :'现场情况',//string
          unfinishedReason :'未完成原因', //string
          emergencyRepair :'转急中',//number 
          needTransgerMeter :'需调表',//number 
          needSecutityCheck :'需安检',//number
          isLock : '是否门锁' //number,
          lockNumber: '门锁编号' //string  
          X: '经度',     // number
          Y: '纬度',     // number
          person:'操作人(施工人员)' //string
          opreateTime :'操作时间(施工日期)',//number utc 
          handleType: '处理类别', //string
          handleContent:'处理内容', //string 
          customerFeedback:'客户反馈' //number
          meterEnergy: '表能量(新表能量)',   // number
          meterIndex: '表指数(拆表指数)',        // number
          sceneType :'现场类别' // number
          refitting :'转改装',//number
          transtoPublic :'转它办',//number 
          replaceResult:'调表结果',//string  --------调表新加字段
          theftGas:'防窃气装置',//number  1:是，0：否
          inspectPerson:'验收人员',//string
          alwaysOpen:'调总开',//string
          oldMeterNo:'旧表表号',//string
          newMeterNo:'新表表号',//string
          newMeterBackNo:'新表备用表号',//string
          deviceNo:'设备号',//string
          meterModel: '型号',   //string
		  caliber: '口径',//string
          meterManufactor: '厂商', 	 //string
		  airDirection:'进气方向',//string
		  surplusMoney:'剩余金额',//string
		  surpluAmount:'剩余用量',//string
		  oldMeterIndex:'旧表指数' //number
		  splitMeterRead:'拆表时抄码' //string
		  inspectNumber:'强检编号'//string
        }
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
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          state :'任务状态', //number 30:接单,40:到场,50:销单
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          state :'任务状态', //number  30:接单,40:到场,50:销单
        },
          ....
      ]
     }
    ```


## 申请挂起  
 
 调表申请挂起

* URL: `v1/mobile/tasks/apply?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
          taskNo :'任务编号',  //string
          taskType :'任务类型',   	//number 3:电话检修  1:调表
          taskSubType :'任务子类型',	 //number 1:漏气维修 2：一般  
          hangUp: '是否挂起', 	 // number 0:不挂 , 1:挂起
     }  
    ```

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: 
          {
          isSuccess: 'true',
          message: '',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          }
     }
    ```


## 工单挂起查询

根据条件查询工单是否挂起

* URL: `v1/mobile/task/query?userId={userId}&taskNo={taskNo}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * taskNo:'任务编号(调表编号)',//string

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          taskNo :'任务编号',  //string
          taskState :'任务状态', //number  10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType :'任务类型',   	//number 3:电话检修  1:调表
          taskSubType :'任务子类型',	 //number 1:漏气维修 2：一般  
          hangUp: '是否挂起', 	 // number 0:不挂 , 1:挂起
         },
         ........
      ]
    }
   ```

## 获取燃气表信息

根据登录人的userid获取燃气表信息

* URL: `v1/mobile/meter/getMetersInfo?userId={userId}`
* METHOD: `GET`

* 参数:
   * userId: '登录人的userid', // number

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          id :'id',  //number
          meterNo :'表号（燃气表条形码）', //string  
          manufactor :'燃气表厂家编号',//string
          backupMeterNo:'备用表号（表本体号）',//string
          model:'表型号',//string
          caliber:'口径',//string
          energy:'表能量',//number
          deviceNo:'设备号',//string
          remark:'备注'//string
         },
         ........
      ]
    }
   ```




## 上传多个文件

上传指定文件。如果文件需要和某一个任务关联需要先上传文件，然后再做关联某一个任务。比如回复任务时有图片需要上传，那么就需要先调用上传接口，上传成功后会返回一个fileId。

* URL: `v1/mobile/files/upload`
* METHOD: `POST`

* 请求内容：

  文件内容, byte数组，支持多个文件同时上传（具体见温州抄表），同时上传文件名

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
        {
          url: '文件url'，
          fileType: '文件类型',
          fileSize: '文件大小',
          fileHash: '文件hash',
          originName: '原始文件名'
        }
        ....
      ]
   }
   ```

* 相同的文件可以反复上传，多个文件上传时，返回都成功或都失败，没有一部分成功或失败情况。
* 服务器上文件会重命名，文件名即fileHash。
* 返回的内容次序和上传文件次序一致。

![demo](http://128.1.3.67:8095/Update/hotline/shanghai/20160928141931.png)

![demo](http://128.1.3.67:8095/Update/hotline/shanghai/20160928143321.png)


## 上传文件关联关系

首先需要先调用文件上传接口，然后调用该方法告知某一个任务和那几个文件做关联。

* URL: `v1/mobile/task/{taskId}/fileInfos/upload`
* METHOD: `POST`

* 参数：
   * taskId: task编号

* 请求内容：

  ```
  {
    [
          {
            url: '文件url'，
            fileType: '文件类型',
            fileSize: '文件大小',
            fileHash: '文件hash'
            fileState:'文件状态' //number  14:用户签字,8:检漏照片, 2:燃气表(安装后燃气表照片),9:故障点,17:门锁,20:交接单,19:旧表在线,12:燃气表气密性检验, 20:自排管气密性
          }
          ....
       ]
    }
    ```

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述'
   }


## 获取电话检修收费项目信息

获取收费项目信息

* URL: `v1/mobile/payItem`
* METHOD: `GET`

* 回复内容：

  ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
       {               
         itemNo: '收费项目编号',//int
         name: '名称',//string
         price: '价格',//number
         specifications:'规格',//string
         unit:'单位',//string
         chargeStandard:'收费标准',//number
         splitStandard:' 拆除收费标准'//number
         remark: '备注'//string
       },
       ...
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
         wordId: '词语id',  int
         parentId: '父级Id', int
         group: '词语组', string
         name: '词语文本', string
         value: '词语值', string
         valueEx: '词语值扩展内容', string
         remark: '备注' string
       },
       ...
     ]
   }
  ```



## 获取IC卡现场制卡、IC卡现场补气、IC卡现场转存单详细信息

获取IC卡现场制卡、IC卡现场补气、IC卡现场转存单详细信息

* URL: `v1/mobile/ic/lists?userNumber={userNumber}&cardNumber={cardNumber}`
* METHOD: `GET`

* 参数:
   * userNumber: '用户编号', // string
   * cardNumber: '表号（表条形码）', //string

* 返回内容：
  ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
       {               
          userNumber ：'用户编号' , //string
          userName: '户名, 	 //string
          address: '地址', 		 //string 
          cardNumber: '表号（表条形码）',   	//string
          meterEnergy: '表能量',		// number
          meterManufactor: '厂家', 	 //string
          price:'当前单价',//number
          gasNature: '用气性质',		//string
		  remarks: '备注', //string
		  limitInfo:'限购信息',//string
		  meterPurchaseValue:'当前表购量',//number
		  money:'金额',//number
		  rechargeRecords:'当前表充值记录',//string list gson字符串
		  meteRecording'调表记录',//string list gson字符串
        },
       ...
     ]
   }
   ```  

## IC卡信息回填
获取IC卡现场制卡、IC卡现场补气、IC卡现场转存信息回填

* URL: `v1/mobile/ic/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
      [
        {
          userNumber ：'用户编号' , //string
          cardNumber: '表号（表条形码）',   	//string
          taskType :'类型',//number 1:ic卡现场制卡,2:IC卡现场补气,3:IC卡现场转存
          replenisReason:'补卡原因',//string
          openFlag:'新开户标志',//number 0:否 1:是
          relpenisFee:'补卡费',//number 0:否 1:是
          remark:'备注',//string
          qiValue:'补气量',//string
          qiMoney:'补气金额',//number
          qiReason:'补气原因',//string
          meterIndex:'表机械指数',//number
          dumpMoney:'转存金额',//number
          dumpGasValue:'转存气量',//string
          historyPrice:'历史单价',//number
          ignore:'忽略',//number 0:否 1:是
          modify:'修改',//number 0:否 1:是
        }
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
          userNumber :'用户编号',  //string
          cardNumber: '表号（表条形码）',   	//string
          extendInfo: 'json string' // 扩展信息, 可为空
          state :'任务状态', //number 30:接单,40:到场,50:销单
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          cardNumber: '表号（表条形码）',   	//string
          extendInfo: 'json string' // 扩展信息, 可为空
          state :'任务状态', //number  30:接单,40:到场,50:销单
        },
          ....
      ]
     }
    ```

## IC卡现场补气审批、IC卡现场转存（转存金额或转存量修改）申请
 
 IC卡现场补气审批、IC卡现场转存（转存金额或转存量修改）申请

* URL: `v1/mobile/ic/applyQi?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
          userNumber :'用户编号',  //string
          cardNumber: '表号（表条形码）',   	//string
          applyApproval: '审批申请', // number 0:申请 , 1:不申请
          type: '业务类型', //2:IC卡现场补气审批申请,3:IC卡现场转存（转存金额或转存量修改）申请
    }  
    ```

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: 
          {
          isSuccess: 'true',
          message: '',
          userNumber :'用户编号',  //string
          cardNumber: '表号（表条形码）',   	//string
          type: '业务类型', //2:IC卡现场补气审批申请,3:IC卡现场转存（转存金额或转存量修改）申请
          extendInfo: 'json string' // 扩展信息, 可为空
          }
     }
    ```


## IC卡现场补气审批申请状态、IC卡现场转存（转存金额或转存量修改）申请状态查询

查询IC卡现场补气审批申请状态

* URL: `v1/mobile/ic/queryQi?userId={userId}&userNumber={userNumber}&type={type}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * userNumber :'用户编号',  //string
   * type: '业务类型', //2:IC卡现场补气审批申请,3:IC卡现场转存（转存金额或转存量修改）申请

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          cardNumber: '表号（表条形码）',   	//string
          approvalState: '审批状态', 	 // number 0:不通过 , 1:通过
         },
         ........
      ]
    }
   ```

##电话检修质检单查询

电话检修质检单查询

* URL: `v1/mobile/telmaintenanceInspect/query?date={date}&office={office}&completeState={completeState}`
* METHOD: `GET`

* 参数:
   * date: '时间', //long utc
   * office:'办事处', // string
   * completeState:'完工状态',//string
* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber ：'用户编号' , //string
          taskNo:'任务编号(调表编号)',   		//string
          taskState: '任务状态',   //number 10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType: '任务类型',   	//number 3:电话检修     1:调表
          taskSubType: '任务子类型',	 //number 1:漏气维修 2：一般  
          cardNumber: '表号(旧表表号)',   	//string
          areaName: '区名',   	 //string
          userLevel: '用户等级',	 //string
          userName: '户名(用户名称)', 	 //string
          address: '地址', 		 //string
          telephone: '电话',  	//string
          mobilePhone: '手机',  	//string
          reflectPeople: '反映人',  	//string
          responseCategory: '反映类别', 	//string
          responseContent: '反映内容', 	//string
          reflectionForm: '反映形式', 	//string
          reactionTime: '反映时间(申请日期)',  	// number, utc
          roadType: '道路类型',		//string
          gasNature: '用气性质',		//string
          infoSources: '信息来源',		//string
          urgency: '紧急程度',		//string
          acceptTime: '接受时间',		// number, utc
          meterEnergy: '表能量(旧表能量)',		// number
          meterIndex: '表指数',				// number
          appointementStart: '预约开始',		//string
          appointementLength: '预约时长',		//string
          appointementEnd: '预约结束',	 //string
          repeat: '重复',   		// number 0:不重复 1：重复 
          barCode: '表条码',		 //string
          correctorBarCode: '校正仪条码',		//string
          settlementTmp: '结算温度',		 // number
          settlementPressure: '结算压力', 		//string
          supplyPressure: '供气压力',		 //string
          pulseEquivalent: '脉冲当量', 		//string
          urgent: '加急', 			// number 0：不加急,1:加急  
          dispatchExtend: '派单备注', 	//string  
          hangUp: '是否挂起', 	 // number 0:不挂 , 1:挂起
          reflectRemarks: '反映备注', //string
          meterModel: '型号(旧表型号)',   //string
          meterManufactor: '厂家(旧表厂商)', 	 //string
          calibratorSupplier: '校正仪供应商',  	//string
          calibratorModel: '校正仪型号', 	 //string
          processLevel: '处理级别', 	 //string
          infoNumber: '信息单号(PDA编号)', 		 //string
          serialNumber: '流水号', 		 //string
          receptionPerson: '受理人',	//string
          userNumber:'用户编号',  //string
          X: '经度',     // number
          Y: '纬度',     // number
          dispatchTime :'派工时间',  //number utc时间
          dispatchPersion:'派工人',  //string
          arriveTime:'到场时间',//long utc
          office:'办事处',//string 
          officeTelphone:'办事处电话',//string
          processTime: '剩余处理时间',//number
         },
         ........
      ]
    }


##电话检修质检单详情查询

电话检修质检单详情查询

* URL: `v1/mobile/telmaintenanceInspect/detail?taskId={taskId}`
* METHOD: `GET`

* 参数:
   * taskId: '工单编号', //string
* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data:
       {
          taskNo :'任务编号',  //string
          taskState :'任务状态', //number  10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType :'任务类型',   	//number 3:电话检修  1:调表
          taskSubType :'任务子类型',	 //number 1:漏气维修 2：一般  
          handleResult :'处理结果(施工结果)',  //string
          handleRemark :'处理备注(退单备注)', //string
          handleProcess :'处理经过', //string
          liveSituation :'现场情况',//string
          unfinishedReason :'未完成原因', //string
          emergencyRepair :'转急中',//number 
          needTransgerMeter :'需调表',//number 
          needSecutityCheck :'需安检',//number
          isLock : '是否门锁' //number,
          lockNumber: '门锁编号' //string  
          X: '经度',     // number
          Y: '纬度',     // number
          person:'操作人(施工人员)' //string
          opreateTime :'操作时间(施工日期)',//number utc 
          handleType: '处理类别', //string
          handleContent:'处理内容', //string 
          customerFeedback:'客户反馈' //number
          meterEnergy: '表能量(新表能量)',   // number
          meterIndex: '表指数(拆表指数)',        // number
          sceneType :'现场类别' // number
          refitting :'转改装',//number
          transtoPublic :'转它办',//number 
         }
    }


##电话检修质检单文件信息查询

电话检修质检单文件信息查询

* URL: `v1/mobile/telmaintenanceInspect/files?taskId={taskId}`
* METHOD: `GET`

* 参数:
   * taskId: '工单编号', //string
* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
           url: '文件url'，
           fileType: '文件类型', 
           fileName: '文件名称',
           fileState:'文件状态' //number  14:用户签字,8:检漏照片, 2:燃气表(安装后燃气表照片),9:故障点,17:门锁,20:交接单,19:旧表在线,12:燃气表气密性检验, 20:自排管气密性
         },
         ........
      ]
    }

## 电话检修质检单处理回填

电话检修质检单处理回填

* URL: `v1/mobile/telmaintenanceInspect/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
      [
        {
          taskNo :'任务编号',  //string
          inspectContent:'质检内容',//string
          qualified:'是否合格',//number 0:不合格,1:合格
          overhaulContent:'检修内容',//string
        }
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
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```

##非居巡检

电话检修质检单查询

* URL: `v1/mobile/nonResidentInspect/lists?userId={userId}`
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber ：'用户编号' , //string
          userName: '户名(用户名称)', 	 //string
          address: '地址', 		 //string
          ch:'统册号',//string
          nature: '性质',		//string
          meterEnergy: '表能量(旧表能量)',		// number
          supplyPressure: '供气压力',		 //string
          icCard:'是否IC卡',//number 1:是 0：否
          lastMeterIndex:'上次抄表指数',//number
          lastCollectionDate:'最近IC卡采集日期',//long utc
          lastInspectDate:'最近巡检日期',//long utc
          lastDistantDate:'最近远传日期',//long utc
          halfYearSales:'半年销售量',//string
          device:'设备',//string
          installPosition:'安装位置',//string
          meterInfo:'表信息',//string
          rechargeInfo:'半年最近充值情况',//string
          lastInspectInfo:'上次巡检信息',//string gson字符串

          },
         ........
      ]
    }

##抄表质检单删除

抄表质检单删除

* URL: `v1/mobile/meterInspect/delete?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
      [
        {
          taskNo :'任务编号',  //string
          delete:'是否删除',//number 0:否，1:是
        }
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
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```



##抄表质检单信息list获取

抄表质检单信息回填
* URL: `v1/mobile/meterInspect/list?userId={userId}`
* METHOD: `GET`

* 参数:
   * userId: 用户id
* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
            userNumber: '用户编号' ,//string
            userName: '用户名' ,//string
            userAddress: '用户地址' ,//string
            meterEnergy  : '表能量', // number 
            st     : '站点', //string 
            ch     : '册本号', //string
            meterTimes  : '抄次', // number
            firstInstallDate : '初装日期', //long  utc
            transferTimeDate : '调表日期', //long  utc
            abnormalMetering : '非正常计量', //string
            gasProperty     : '用气性质', //string
            gasSource    : '气源', //string
            epitope     : '表位', //string
            alternateMeterNo     : '备用表号', //string
            contactPersion     : '联系人', //string
            contactTel     : '联系电话', //string
            contactCellPhone     : '联系手机', //string
            area     : '所属地区', //string
            gasUsage     : '用气用途', //string
            jane     : '简号', //string
            collectPressurediff  : '是否收取压力差', // number
            arrearsInfo     : '欠费信息', //string
            ParallelGroup     : '并联组号', //string
            newMeterCode  : '新表底码', // number
            isREsidents  : '是否居民', // number
            oldMeterRead  : '旧表抄码', // number
            lastMeterRead  : '上次抄码', // number
            isReplace  : '是否换表', // number
            pressuredifCoeffic  : '压力差系数', // number 
            patchType  : '补收类型', // number
            indicationError  : '示值误差', // number
            generFailureMaintenance  : '一般失效补收', // number
            compensationMode  : '补偿方式0：不收补偿费；1：固定值每月；2：表能量乘补偿费系数', // number
            gasSourceValue  : '气源1：人工煤气；2：天然气', // number
            shuntSerialNumber  : '并联序号', // number
            abMeasureSupplement  : '非正常计量补收量', // number
            lastWork  : '最后工次', // number
            cost  : '费用', // number
            lastConsumption  : '上次消费量', // number
            lastMeterDate  : '上次抄表日期', // long 
            lastMeterCycle  : '上次抄表周期，天数', // number
            hignALowCoeffcie  : '册本的量高量低系数', // number
            highAndLowSign  : '量高量低标志;1:量高;-1:量低', // number
            serialNumber  : '册内序号', // number
            meterNo     : '表号', //string
            lastMeterStatus     : '上次抄表状态代码', //string
            replacedNotBackFill  : '已换表未回填，1：已换表未回填', // number
            popMoreThanOne  : '一户多人口标识', // number
            compenApplication  : '补收申请（已通过）', // number
            meterMaxRead  : '表最大读数', // number
            gasPoperty  : '用气性质5,8,9为1，否则为0', // number
            roundRobin  : '轮转', // number
            illegal  : '违章，1：有违章', // number
            needPictures  : '是否需要拍照，1：需要；0：不需要', // number
            meterMethod     : '抄表方式 5：IC, 8：无线直读 9：自抄 10：停用', //string
            groupId  : '组号', // string
            newSecret:'新密钥',//string
            oldsecret:'旧密钥',//string
            dataResource:'数据来源',//string
            dataResourceValue:'采集数据',//number
            dataResourceTime:'采集数据时间',//long
            publicWelfare:'公益性用户 ,1：是,0：否',//number
            lastMeterRemark:'上次抄表备注',//string
            selfReport:'微客服自报',//number 
            cumulants:'年累积量',//number 
            bluetoothDevice:'蓝牙设备',//String
            extend  :'扩展字段' //string
         },
         ........
      ]
    }
##抄表质检单信息回填

抄表质检单信息回填

* URL: `v1/mobile/meterInspect/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 用户id

* 请求内容:

    ```
    {
      [
        {
          taskNo :'任务编号',  //string
          taskState: '任务状态',//number 50:销单
          isLock : '是否门锁',//number 1:门锁 0：进门
          index: '指数',// number
          meterSituation:'抄表情况',//string
        }
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
          taskState: '任务状态',//number 50:销单
          message: '',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          taskState: '任务状态',//number 50:销单
          message: '失败原因',
          taskNo :'任务编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```

## 获取调表单信息接口

获取调表单信息接口

* URL: `v1/mobile/leader/adjustTask/{barCode}`
* METHOD: `GET`

* 参数:
    * barCode: 调表单条码

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
         "adjustNumber": "调表编号", // string     
         "adjustBarCode": "调表单条码", // string
         "name":  "户名" //  string
         "address": "地址", // string
		 "adjustState": "状态", // number 例如：0：未派单 1：已派单 2：已退单 3：已销单      
      }
   }
   ```

##派单信息回填

派单派表信息回填

* URL: `v1/mobile/leader/dispatchTask?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 施工队长id

* 请求内容:

    ```
    {
      [
        {
          adjustNumber :'调表编号',  //string
          workId : '施工人员id',//number （施工人员信息通过词语接口下载）
          workName: '施工人员',//string
          remark:'派单备注',//string
        }
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
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```****

##派表信息回填

派表信息回填

* URL: `v1/mobile/leader/dispatchMeter?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 施工队长id

* 请求内容:

    ```
    {
      [
        {
          meterNumber :'燃气表条码',  //string
          workId : '施工人员id',//number （施工人员信息通过词语接口下载）
          workName: '施工人员',//string
          remark:'派单备注',//string
        }
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
          meterNumber :'燃气表条码',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          meterNumber :'燃气表条码',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```****

## 获取燃气表信息接口

获取燃气表信息接口

* URL: `v1/mobile/leader/adjustMeter/{gasMeterBarCode}`
* METHOD: `GET`

* 参数:
    * gasMeterBarCode: 燃气新表条码

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
         "gasMeterBarCode": "燃气表条码", // string     
         "meterEnergy": "表能量", // number
         "meterType":  "表型号" //  string
		 "meterState": "状态", // number 例如：1在库，2在途，3领用在途，4在线，5下线，6报废，7报损,-1送厂被替换
      }
   }
   ```

## 下载门锁数据

根据条件获取工单数据

* URL: `v1/mobile/leader/lock/list?userId={userId}
* METHOD: `GET`

* 参数:
   * userId: '施工队长id', // number

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber ：'用户编号' , //string
          adjustNumber :'调表编号',  //string
          userName:'用户名称',      //string
          userAddress: '用户地址',   //string
          taskType: '任务类型',   	//number
          taskTypeName: '任务类型（string）',  //string
         },
         ........
      ]
    }

## 门锁处理上传

处理多条门锁

* URL: `v1/mobile/leader/lock/process?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: '施工队长id', // number

* 请求内容:

    ```
    {
      [
        {
           adjustNumber :'调表编号',  //string
           userNumber ：'用户编号' , //string
           lockNumber ：'门锁编号' , //string
           remark ：'备注' , //string
        }
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
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```

## 获取队长退单信息接口

根据条件获取退单数据

* URL: `v1/mobile/leader/back/list?userId={userId}&workId={workId}&dispatchData={dispatchData}`
* METHOD: `GET`

* 参数:
   * userId: '队长id', // number
   * workId: '施工人员id', // number 
   * dispatchData: '派单日期', // number  

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
         "adjustNumber": "调表编号", // string     
         "adjustBarCode": "调表单条码", // string
         "name":  "户名" //  string
         "address": "地址", // string
		 "adjustState": "状态", // number 例如：0：未派单 1：已派单 2：已退单 3：已销单  
         "remark": "备注", // string
         },
         ........
      ]
    }


## 队长退单确认信息回填

退单信息回填

* URL: `v1/mobile/leader/back?userId={userId}`
* METHOD: `POST`

* 参数:
   * userId: 施工队长id

* 请求内容:

    ```
    {
      [
        {
          adjustNumber :'调表编号',  //string
          gasMeterBarCode: '燃气表条码',//string 
          workId : '施工人员id',//number （施工人员信息通过词语接口下载）
          workName: '施工人员',//string
          remark:'队长退单确认备注',//string
        }
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
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          adjustNumber :'调表编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          ....
      ]
     }
    ```


## 消息定义

消息推送主要采用RabbitMQ搭建，加入Mqtt协议插件。
消息内容为JSON字符串。具体内容根据消息类型不同而变化。

消息类型包含：

* 新工单, 201

### 新工单

当新的工单被派遣给指定人后发生。一次消息内可能携带多个工单数据（视内容而定）。

    ```
    {
      type: '201', // number
      content: [ // 任务内容, array
        {
           userNumber ：'用户编号' , //string
          taskNo:'任务编号(调表编号)',   		//string
          taskState: '任务状态',   //number 10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType: '任务类型',   	//number 3:电话检修     1:调表
          taskSubType: '任务子类型',	 //number 1:漏气维修 2：一般  
          cardNumber: '表号(旧表表号)',   	//string
          areaName: '区名',   	 //string
          userLevel: '用户等级',	 //string
          userName: '户名(用户名称)', 	 //string
          address: '地址', 		 //string
          telephone: '电话',  	//string
          mobilePhone: '手机',  	//string
          reflectPeople: '反映人',  	//string
          responseCategory: '反映类别', 	//string
          responseContent: '反映内容', 	//string
          reflectionForm: '反映形式', 	//string
          reactionTime: '反映时间(申请日期)',  	// number, utc
          roadType: '道路类型',		//string
          gasNature: '用气性质',		//string
          infoSources: '信息来源',		//string
          urgency: '紧急程度',		//string
          acceptTime: '接受时间',		// number, utc
          meterEnergy: '表能量(旧表能量)',		// number
          meterIndex: '表指数',				// number
          appointementStart: '预约开始',		//string
          appointementLength: '预约时长',		//string
          appointementEnd: '预约结束',	 //string
          repeat: '重复',   		// number 0:不重复 1：重复 
          barCode: '表条码',		 //string
          correctorBarCode: '校正仪条码',		//string
          settlementTmp: '结算温度',		 // number
          settlementPressure: '结算压力', 		//string
          supplyPressure: '供气压力',		 //string
          pulseEquivalent: '脉冲当量', 		//string
          urgent: '加急', 			// number 0：不加急,1:加急  
          dispatchExtend: '派单备注', 	//string  
          hangUp: '是否挂起', 	 // number 0:不挂 , 1:挂起
          reflectRemarks: '反映备注', //string
          meterModel: '型号(旧表型号)',   //string
          meterManufactor: '厂家(旧表厂商)', 	 //string
          calibratorSupplier: '校正仪供应商',  	//string
          calibratorModel: '校正仪型号', 	 //string
          processLevel: '处理级别', 	 //string
          infoNumber: '信息单号(PDA编号)', 		 //string
          serialNumber: '流水号', 		 //string
          receptionPerson: '受理人',	//string
          userNumber:'用户编号',  //string
          X: '经度',     // number
          Y: '纬度',     // number
          dispatchTime :'派工时间',  //number utc时间
          dispatchPersion:'派工人',  //string
          arriveTime:'到场时间',//long utc
          office:'办事处',//string 
          officeTelphone:'办事处电话',//string
          processTime: '剩余处理时间',//number
          lastOperateTime:'最近一次调表日期',number utc ------调表新加字段
          replaceReason:'换表原因',//string 
          spareNo:'备用表号',//string
		  diffValue:'差值',//string
		  caliber: '口径',//string
		  meterRead:'旧表抄码',//number
          installTime:'安装日期',//number utc 
          hangUpValue:'倒挂气量',//number
          AlarmValue:'报警气量'//number 
        },
        ...
        { ... },
        { ... }
      ]
    }
