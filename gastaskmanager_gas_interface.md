# 服务端接口定义

> 版本：v1.0
> 作者：zang


## 修订记录

1. v1.0, 2017-09-05, zang, 初始版本。

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
      appIdentify: 'com.sh3h.gastask' 
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
      }
   }
   ```

## 下载工单数据

根据条件获取工单数据

* URL: `v1/mobile/tasks/list?userId={userId}&type={type}
* METHOD: `GET`

* 参数:
   * userId: '用户id', // number
   * type: '业务类型', // number 1:电话维修

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber ：'用户编号' , //string
          taskNo:'任务编号',   		//string
          taskState: '任务状态',   //number 10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType: '任务类型',   	//number 3:电话检修  
          taskSubType: '任务子类型',	 //number 1:漏气维修 2：一般  
          cardNumber: '表号',   	//string
          areaName: '区名',   	 //string
          userLevel: '用户等级',	 //string
          userName: '户名', 	 //string
          address: '地址', 		 //string
          telephone: '电话',  	//string
          mobilePhone: '手机',  	//string
          reflectPeople: '反映人',  	//string
          responseCategory: '反映类别', 	//string
          responseContent: '反映内容', 	//string
          reflectionForm: '反映形式', 	//string
          reactionTime: '反映时间',  	// number, utc
          roadType: '道路类型',		//string
          gasNature: '用气性质',		//string
          infoSources: '信息来源',		//string
          urgency: '紧急程度',		//string
          acceptTime: '接受时间',		// number, utc
          meterEnergy: '表能量',		// number
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
          meterModel: '型号',   //string
          meterManufactor: '厂家', 	 //string
          calibratorSupplier: '校正仪供应商',  	//string
          calibratorModel: '校正仪型号', 	 //string
          processLevel: '处理级别', 	 //string
          infoNumber: '信息单号', 		 //string
          serialNumber: '流水号', 		 //string
          receptionPerson: '受理人',	//string
          userNumber:'用户编号',  //string
          X: '经度',     // number
          Y: '纬度',     // number
          dispatchTime :'派工时间',  //number utc时间
          dispatchPersion:'派工人'  //string
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
          taskState :'任务状态', //string  10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType :'任务类型',   	//number 3:电话检修  
          taskSubType :'任务子类型',	 //number 1:漏气维修 2：一般  
          handleResult :'处理结果',  //string
          handleRemark :'处理备注', //string
          handleProcess :'处理经过', //string
          liveSituation :'现场情况',//string
          handlerTimely :'处理及时',//number 
          eliminateTimely :'销件及时',//number 
          unfinishedReason :'未完成原因', //string
          repeatMark :'重复标记',//number 
          emergencyRepair :'转急修',//number 
          needTransgerMeter :'需调表',//number 
          needSecutityCheck :'需安检',//number
          isLock : '是否门锁' //number,
          lockNumber: '门锁编号' //string  
          X: '经度',     // number
          Y: '纬度',     // number
          person:'操作人' //string
          opreateTime :'操作时间',//number utc 
          handleType: '处理类别',
          handleContent:'处理内容',
          customerFeedback:'客户反馈'
          meterEnergy: '表能量',   // number
          meterIndex: '表指数',        // number
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
          taskNo: '任务编号', // number
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          taskNo: '任务编号', // number
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
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
          taskNo:'任务编号',      //string
          taskState: '任务状态',   //number 10:接受,20:派单,30:接单,40:到场,50:销单,-10:退单
          taskType: '任务类型',     //number 3:电话检修  
          taskSubType: '任务子类型',  //number 1:漏气维修 2：一般  
          cardNumber: '表号',     //string
          areaName: '区名',      //string
          userLevel: '用户等级',   //string
          userName: '户名',    //string
          address: '地址',     //string
          telephone: '电话',    //string
          mobilePhone: '手机',    //string
          reflectPeople: '反映人',   //string
          responseCategory: '反映类别',   //string
          responseContent: '反映内容',  //string
          reflectionForm: '反映形式',   //string
          reactionTime: '反映时间',   // number, utc
          roadType: '道路类型',   //string
          gasNature: '用气性质',    //string
          infoSources: '信息来源',    //string
          urgency: '紧急程度',    //string
          acceptTime: '接受时间',   // number, utc
          meterEnergy: '表能量',   // number
          meterIndex: '表指数',        // number
          appointementStart: '预约开始',    //string
          appointementLength: '预约时长',   //string
          appointementEnd: '预约结束',   //string
          repeat: '重复',       // number 0:不重复 1：重复 
          barCode: '表条码',    //string
          correctorBarCode: '校正仪条码',    //string
          settlementTmp: '结算温度',     // number
          settlementPressure: '结算压力',     //string
          supplyPressure: '供气压力',    //string
          pulseEquivalent: '脉冲当量',    //string
          urgent: '加急',       // number 0：不加急,1:加急  
          dispatchExtend: '派单备注',   //string  
          hangUp: '是否挂起',    // number 0:不挂 , 1:挂起
          reflectRemarks: '反映备注', //string
          meterModel: '型号',   //string
          meterManufactor: '厂家',   //string
          calibratorSupplier: '校正仪供应商',   //string
          calibratorModel: '校正仪型号',    //string
          processLevel: '处理级别',    //string
          infoNumber: '信息单号',      //string
          serialNumber: '流水号',     //string
          receptionPerson: '受理人', //string
          user_number:'用户编号',  //string
          X: '经度',     // number
          Y: '纬度',     // number
          dispatchTime :'派工时间',  //number utc时间
          dispatchPersion:'派工人'  //string
        },
        ...
        { ... },
        { ... }
      ]
    }
