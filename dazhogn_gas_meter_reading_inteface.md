# 服务端接口定义

> 版本：v1.1
> 作者：zang


## 修订记录

1. v1.0, 2017-09-25, zang, 初始版本。
2. v1.1, 2017-12-01, zang, 调整接口。
3. v1.2, 2017-12-12, 下载上传接口新加字段。
4. v1.3, 2017-12-27, 新加获取校正仪录入信息,修改用气历史信息下载接口
5. v1.4, 2018-1-22,用户基本信息，抄表数据上传接口新加字段

## 概述

本文主要定义任务移动端接口定义规范。


## 登录接口

登录

* URL: `v1/mobile/auth`
* METHOD: `POST`

* 请求内容:
    ```
    {
      account: '领用人账号',
      pwd: '领用人密码',
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

* URL: `v1/mobile/user/{account}`
* METHOD: `GET`

* 参数:
    * account: '领用人账号'

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

## 册本信息获取

* URL: `v1/mobile/volume/{account}`
* METHOD: `GET`

* 参数:
    * account: 领用人账号

* 返回内容:
 
 ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
              account: '登录账号' ,//string
              st     : '站点', //string 
              ch     : '册本号', //string
              lastReadDate  : '上次抄表日期', //long utc
              readDate  : '本次抄表日期', //long  utc
              nextReadDate  : '本次抄表日期', //long  utc
              workTime  : '工次', // number 
              userNo  : '领用编号', // string 
              accountYear  : '账务年月', // number 
              receiptDate  : '领用日期', //long  utc
              meterReadSchedule  : '抄表日程', //long  utc
              extendedDays  : '允许的超期天数', // number 
              meterTeamLeader: '抄表组长' ,//string
              teamLeaderPwd: '抄表组长密码' ,//string
              cebenlgldxs  : '册本量高量低系数', // number 
              gongBanLGLDXX  : '工办量高量低系数', // number 
              minBanLGLDXXs  : '民办量高量低系数', // number
              lgLdHB  : '量高量低分析环比', // number 
              lgLdTB  : '量高量低分析同比', // number
              extend  :'扩展字段' //string
         }
    }

   ```

## 用户基本信息下载

* URL: `v1/mobile/meter/list?account={account}`
* METHOD: `GET`

* 参数:
    * account : 登录账号  //string 


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
              meterMethod     : '抄表方式 5：IC, 8：1.0 无线直读,10：2.0无线蓝牙抄表,20：自抄,30:停用' , //string
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
              correctorName:'校正仪号', //string 为空则不需要校正仪录入 
              stopSource:'停抄来源',//string 
              stopType:'停抄类型',//string
              },
         ........
      ]
    }
     ```

## 抄表分析下载
* URL: `v1/mobile/meterAlysis/list?account={account}`
* METHOD: `GET`

* 参数:
    * account : 登录账号  //string 


* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
            st     : '站点', //string 
            ch     : '册本号', //string
            userNumber: '用户编号' ,//string 
           	volumeNo: '册本号' ,//string
          	alysisYearAndMonth: '分析年月' ,//number
          	alysisType: '分析类型' ,//number
          	meterSecond: '抄次' ,//number
          	analysisRatio: '分析比率' ,//number
          	shangyueryl: '上月日用量/去年同期日用量' ,//number
          	lastReadCycle: '上月抄表周期/去年抄表周期' ,//number
          	dailyUseOfthis: '本月日用量' ,//number
         	meterCycleofthis: '本月抄表周期' ,//number
         	lastMeterDate: '上次抄表日期' ,//long utc 
          	ratio: '比率' ,//number
         	oneLevelReason: '一级原因' ,//number
         	twoLevelReason: '二级原因' ,//number
         	remarks: '备注' ,//string
         	alysisDate: '分析日期' ,//long utc   
       	 },
         ........
      ]
    }
     ```

## 用户分组数据获取

* URL: `v1/mobile/meter/groupList?account={account}`
* METHOD: `GET`

* 参数:
    * account : 登录账号  //string 


* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
            id     : '序号', //string 
            st     : '站点', //string  
            ch     : '册本号', //string
            groupID     : '组号', //string 唯一
            address     : '位置', //string
            totalNumber : '数量', //number
            extend  :'扩展字段' //string
         }
       ]
    }
   ```

## 获取抄表备注

* URL: `v1/mobile/meter/remarks?account={account}`
* METHOD: `GET`

* 参数:
    * account : 登录账号  //string 

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
            id     : '序号', //number  
            userNumber: '用户编号' ,//string
            remark : '备注', //string
            remarkFlag     : '备注类别', //number 0:短期备注 1:长期备注
            operater : '操作人', //string
            operateTime  :'操作时间' //long utc 
         }
       ]
    }
   ```


## 获取册本状态

* URL: `v1/mobile/meter/volumeState?account={account}`
* METHOD: `GET`

* 参数:
    * account : 登录账号  //string 

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
            st     : '站点', //string  
            ch     : '册本号', //string
            state  : '状态',//number 10:'未下载',20'已下载',30:'已提交'
            canDownload:'能下载',//number 0:'可以',1:'不能' 
           }
       ]
    }
   ```




## 抄表数据上传

* URL: `v1/mobile/meter/process?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          userNumber: '用户编号' ,//string
          meterRead  : '本次抄码', // number
          meterStatus: '抄表状态' ,//string
          consumption  : '消费量', // number
          vacantCapacityFee  : '空置容量费', // number
          PatchType  : '补收类型', // number
          amountOfSupplement  : '补收量', // number
          oldConsumption  : '旧表消费量', // number
          isOpenForm  : '是否开换表单 0：不开；1：开', // number
          replaceMethond  : '换表方式1:旧表已开;2:调表补收', // number
          estimationMethod  : '估算方式', // number
          supplementaryMode  : '补收方式', // number
          newMeterConsumption  : '新表消费量', // number
          presureDiff  : '压力差', // number
          abMeasureSupplement  : '非正常计量补收量', // number
          remarks: '备注' ,//string
          meterDate: '抄表时间' ,//long utc 
          suppModeValue  : '补收方式对应输入值', // number 
          estiMethodValue  : '估计方式对应输入值', // number
          rotate  : '是否轮转0：否；1：是', // number
          blueReadRecord  : '直读记录', // number
          recoveryMeter  : '恢复抄表 ', // number (1:临时、2：正常 )
          userNo  : '领用编号', // string 
          meterState:'表的状态',//number 
          setSecreteFlag:'密钥修改标志 (1:修改成功)',//number 
          saveNewSecrete: '新密钥', // string 
          saveOldSecrete: '旧密钥', // string 
          x:'经度',//number
          y:'纬度',//number
          wifiReadState:'直读状态',//number 1：直读,0:否 
          voltage：'电压',//string
          phoneSerialNumber:'手机序列号',//string
          lastMeterRead  : '上次抄码', // number 
          meterNo     : '表号', //string
          meterMethod     : '抄表方式 5：IC, 8：1.0 无线直读,10：2.
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```

##提交

* URL: `v1/mobile/submit/?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {  
        st     : '站点', //string  
        ch     : '册本号', //string   
        highVolume :'量高',//number
        lowVolume :'量低',//number
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
          st     : '站点', //string  
          ch     : '册本号', //string   
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因', 
          st     : '站点', //string  
          ch     : '册本号', //string   
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```






## 停抄、补收 报修申请上传

* URL: `v1/mobile/meter/?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {  
          userNumber: '用户编号' ,//string
          applyType: '申请类型' ,//number 1：停抄申请、2：补收申请:3：报修申请
          stopMeterSource: '停抄申请来源 ' ,//number  词语值 
          stopMeterType: '停抄类型 ' ,//number   词语值
          stopMeterReason: '补收原因   ' ,//number 词语值
          chargeAginAmount: '补收量' ,//number 词语值
          chargeAginType: '补收类型' ,//number 词语值
          opreateTime: '处理时间' ,//long utc
          userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```


## 用户分组上传

* URL: `v1/mobile/meter/?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {  
            id     : '序号', //string 
            st     : '站点', //string  
            ch     : '册本号', //string
            groupID     : '组号', //string 唯一
            address     : '位置', //string
            totalNumber : '数量', //number
            userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```


## 校正仪录入信息上传

* URL: `v1/mobile/meter/correctorInput?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          userNumber: '用户编号' ,//string
          meterRead  : '表头读数', // number
          preCorrectFlow: '校正前流量' ,//number
          correctedFlow  : '校正后流量', // number
          WorkingPressure  : '工作压力', // number
          workingTemp  : '工作温度', // number
          zValue  : 'Z值', // number
          batteryLife  : '电池寿命', // number
          faultFlow  : '故障流量', // number
          instantTraffic  : '即时流量', // number
          measuredPressure  : '实测压力', // number
          refuelingDate  : '加油日期', // long utc 
          dischargeDate  : '放水日期', // long utc 
          remarks: '备注' ,//string
          userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```


## 违章信息录入上传

* URL: `v1/mobile/meter/illegal?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          userNumber: '用户编号' ,//string
          violation: '违章情况,违章情况编码以逗号分隔，例如：1,2,3' ,//string
          issueDate: '发单日期' ,//string
          hairSole  : '发单人', // long utc 
          remarks: '备注' ,//string
          userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```


## 非正常计量输入信息上传

* URL: `v1/mobile/meter/abnormalMeasure?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          userNumber: '用户编号' ,//string
          gasUtilization: '用气情况,用气情况编码以逗号分隔，例如：1,2,3' ,//string
          opreateTime: '操作时间' ,//long utc 
          remarks: '备注' ,//string
          userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```


## 抄表分析上传

* URL: `v1/mobile/meter/meterAlysis?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          userNumber: '用户编号' ,//string 
          volumeNo: '册本号' ,//string
          alysisYearAndMonth: '分析年月' ,//number
          alysisType: '分析类型' ,//number
          meterSecond: '抄次' ,//number
          analysisRatio: '分析比率' ,//number
          shangyueryl: '上月日用量/去年同期日用量' ,//number
          lastReadCycle: '上月抄表周期/去年抄表周期' ,//number
          dailyUseOfthis: '本月日用量' ,//number
          meterCycleofthis: '本月抄表周期' ,//number
          lastMeterDate: '上次抄表日期' ,//long utc 
          ratio: '比率' ,//number
          oneLevelReason: '一级原因' ,//number
          twoLevelReason: '二级原因' ,//number
          remarks: '备注' ,//string
          alysisDate: '分析日期' ,//long utc 
          userNo  : '领用编号', // string
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
          analysisType :'分析类型',// number int
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          analysisType :'分析类型',// number  int
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```

## 催款单数据上传

* URL: `v1/mobile/meter/reminderUpload?account={account}`
* METHOD: `POST`

* 参数:
  account:'领用人账号'


* 请求内容:

    ```
    {
      [
        {
          reminderTime: '催款单条码' ,//string
          confirmTime: '确认时间' ,//long utc 
          x: '经度' ,//number
          y: '纬度' ,//number
          userNo  : '领用编号', // string
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
          extendInfo: 'json string' // 扩展信息, 可为空
          },
          {
          isSuccess: 'false',
          message: '失败原因',
          userNumber :'用户编号',  //string
          extendInfo: 'json string' // 扩展信息, 可为空
        },
          ....
      ]
     }
    ```






## 获取违章情况

* URL: `v1/mobile/meter/violation`
* METHOD: `GET`

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          violationNo :'违章情况编码',  //string
          violationContent :'违章情况描述',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
   ```

## 获取用气情况

* URL: `v1/mobile/meter/gasUtilization`
* METHOD: `GET`

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          gasUtiName :'参数名称',  //string
          gasUtiValue :'参数值',  //string
          gasUtiNO :'参数编号',  //number
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```

## 获取抄表状态

* URL: `v1/mobile/meter/meterStatus`
* METHOD: `GET`

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          id :'ID',  //number
          parentStateCode :'上级状态编码',  //number
          codeLevel :'编码级别',  //number
          meterState :'抄表状态',  //string
          stateName :'状态名称',  //string
          pcShortCutKey :'PC快捷键',  //string
          meterAlgorithm :'抄表算法编码',  //string      
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```

## 获取补偿系数

* URL: `v1/mobile/meter/compCoefficient`
* METHOD: `GET`

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          compCoefficient :'补偿系数',  //number
          compmethond :'补偿方式',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取词语信息（分析原因、补收原因、补收类型、停抄来源、停抄类型）

* URL: `v1/mobile/meter/analysisReason`
* METHOD: `GET`

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          wordsContent :'词语内容',  //string
          wordsValue :'词语值',  //number
          wordsRemarks :'词语备注',  //number
          parentWordsNo :'隶属词语编号',  //number
          extend  :'扩展字段' //string
         },
         .......
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

* URL: `v1/mobile/meter/{account}/{userNumber}/fileInfos/upload`
* METHOD: `POST`

* 参数：
   * account : 领用人账号
   * userNumber: 用户编号


* 请求内容：

  ```
  {
    [
          {
            url: '文件url'，  
            fileType: '类型:校正仪录入；违章情况',
            fileName: '文件名',
            fileHash: '文件hash',
            pictureType : 拍照对象类型 抄表数据:154,校正仪输入:157,量高量低分析:155,非正常计量输入:156,违章信息:158
            userNo  : '领用编号', // string
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
    ```



## 获取用气历史信息

* URL: `v1/mobile/meter/historyByAccount?account={account}`
* METHOD: `GET`

* 参数:
    * account: 领用人账号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          meterReadingCycle :'上三月抄表周期/去年同期抄表周期',  //string
          totalConsumption :'上三月消费总量/去年同期消费总量',  //number
          dailyDose :'上三月日均用量/去年同期日均用量',  //number
          gasHistoryType :'类型0：上三月；1：去年同期',  //number
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
   ```

## 获取用气历史信息

根据用户编号获取用气历史信息

* URL: `v1/mobile/meter/history?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    *  userNumber:'用户编号' //string

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          meterReadingCycle :'上三月抄表周期/去年同期抄表周期',  //string
          totalConsumption :'上三月消费总量/去年同期消费总量',  //number
          dailyDose :'上三月日均用量/去年同期日均用量',  //number
          gasHistoryType :'类型0：上三月；1：去年同期',  //number
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
   ```

## 获取设备信息

* URL: `v1/mobile/meter/equipmentInfo?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          equipmentName :'设备名称',  //string
          specifications :'规格',  //string
          timeFlow :'时流量',  //number
          number :'数量',  //number
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```

## 获取抄表历史信息

* URL: `v1/mobile/meter/meterHistory?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          meterStatus :'抄表状态',  //string
          timeFlow :'账务年月',  //long utc 
          meterFrequency :'抄次',  //number
          meterRead :'本次抄码',  //number
          totalConsumption :'消费总量',  //number
          annualTotalConsump :'年消费总量',  //number
          remarks :'备注',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```



## 获取报警器信息

* URL: `v1/mobile/meter/alarmInfo?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          alarmNumber :'报警器编号',  //string
          alsrmObject :'报警器对象',  //string
          administrators :'管理员',  //string
          supplier :'供应商',  //string
          installationLocation :'安装位置',  //string
          detectorQuantity :'探测器数量',  //number
          detectorModel :'探测器型号',  //string
          shutOffValveLocation :'切断阀安装位置',  //string
          shutOffValveCaliber :'切断阀口径',  //string
          installDate :'安装日期',  //long utc 
          gasDeviceUseSituation :'燃气设备使用情况',  //string       
          alarmInstallStatus :'报警器安装状态',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```



## 获取报警器年检信息

* URL: `v1/mobile/meter/alarmYearlyInspection?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          alarmNumber :'报警器编号',  //string
          yearlyinspecDate :'年检日期',  //long utc
          nianjiandw :'年检单位',  //string
          nianjianjg :'年检结果',  //string
          nianjiandqrq :'年检到期日',  //long utc
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取报警器保养信息

* URL: `v1/mobile/meter/maintainInfo?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          alarmNumber :'报警器编号',  //string
          contractMaturityDate :'合同到期日期',  //long utc
          maintenanceUnit :'保养单位',  //string
          dispatchType :'派工类型',  //string
          dispatchPerson :'发放人',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取报警系统派工明细

* URL: `v1/mobile/meter/maintainInfopgmx?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          dispatchType :'派工类型',  //string
          dispatchTime :'派工时间',  //long utc
          dispatchContent :'派工内容',  //string
          dispatchPerson :'发放人',  //long utc 
          grantDate :'发放日期',  //long utc 
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取校正仪信息

* URL: `v1/mobile/meter/calibratorInfo?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          correctorNumber :'校正仪号',  //string
          correctorModel:'校正仪型号',  //string
          supplier :'供应商',  //string
          cpValue :'cp值',  //number
          maxPresure :'最大压力',  //number
          minPresure :'最小压力',  //number
          settlementTemp :'结算温度',  //number
          settlementPresure :'结算压力',  //number
          supplyPressure :'供气压力',  //number     
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取燃气表信息

* URL: `v1/mobile/meter/gasmeter?userNumber={userNumber}`
* METHOD: `GET`
* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          meterNo :'表号',  //string
          backupMeterNO:'备用表号',  //string
          antiTheftDevice :'防窃器装置',  //string
          dataAcquisiMode :'数据采集方式',  //string
          meterMethod :'抄表方式',  //string
          installMethod :'安装方式',  //string
          meterModel :'表型',  //string
          holesNo :'孔数',  //number      
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```


## 获取调表记录

* URL: `v1/mobile/meter/transferRecords?userNumber={userNumber}`
* METHOD: `GET`

* 参数:
    * userNumber: 用户编号

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber :'用户编号',  //string
          oldEnergy :'旧表能量',  //number
          oldIndex:'旧表指数',  //number
          oldMeterSupplir :'旧表生产商',  //string
          oldMeterNo :'旧表表号',  //string
          oldMeterModel :'旧表型号',  //string
          oldMeterAlternate :'旧表备用表号',  //string
          oldMeterPadNo :'旧表PDA编号',  //string
          oldCaliber:'旧表口径',  //string
          oldInstallMethond:'旧表安装方式',  //string
          oldHoles :'旧表孔数',  //number
          newMeterCode:'新表底数',  //string
          newModel :'新表型号',  //string
          extend  :'扩展字段' //string
         },
         .......
      ]
    }
    ```

## 获取校正仪录入信息

* URL: `v1/mobile/meter/corrector/list?account={account}`
* METHOD: `GET`

* 参数:
    * account: '领用人账号',//string

* 返回内容：

    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
          userNumber: '用户编号' ,//string
          meterRead  : '表头读数', // number
          preCorrectFlow: '校正前流量' ,//number
          correctedFlow  : '校正后流量', // number
          WorkingPressure  : '工作压力', // number
          workingTemp  : '工作温度', // number
          zValue  : 'Z值', // number
          batteryLife  : '电池寿命', // number
          faultFlow  : '故障流量', // number
          instantTraffic  : '即时流量', // number
          measuredPressure  : '实测压力', // number
          refuelingDate  : '加油日期', // long utc 
          dischargeDate  : '放水日期', // long utc 
          remarks: '备注' ,//string
         },
         .......
      ]
    }
    ```



## 消息定义

消息推送主要采用RabbitMQ搭建，加入Mqtt协议插件。
消息内容为JSON字符串。具体内容根据消息类型不同而变化。

消息类型包含：
     待定







