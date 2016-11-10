# 服务端接口定义

> 版本：v1.1  
> 作者：zangwei

## 修订记录

1. v1.0, 2016-10-31, zangwei, 初始版本
2. v1.1, 2016-11-3, zangwei, 调整部分接口


## 概述
本文主要定义热线工单移动端接口定义规范。

## 约定

本文遵循___[敏捷平台接口规范](api_common.md)___文档的约定。

## App更新

* URL：`v1/mobile/update/app/check?appId={appId}&ver={ver}`
* METHOD：`GET`
* 请求参数：
    * appId，string，application id(如：com.sh3h.meterreading）
    * ver，int，版本号

* 返回内容：
    > 服务端通过比较本地数据库中app版本和请求的版本，返回最新的app版本，或无版本返回（data为null？）。

    ```
    {
        code: 0,
        statusCode: 200, // http请求返回值
        message: '操作描述',
        data: {
            ver: 10,
            url: 'http://example.com/publish/app/app_id_app_v12.zip'
        }
    }
    ```

## 数据包更新

* URL：`v1/mobile/update/data/check?appId={appId}&ver={ver}`
* METHOD：`GET`
* 请求参数：
    * appId，string，application id(如：com.sh3h.meterreading）
    * ver，int，版本号

* 返回内容：
    > 服务端通过比较本地数据库中data版本和请求的版本，返回最新的data版本，或无版本返回（data为null？）。

    ```
    {
        code: 0,
        statusCode: 200, // http请求返回值
        message: '操作描述',
        data: {
            ver: 10,
            url: 'http://example.com/publish/data/app_id_data_v12.zip'
        }
    }
    ```


## 检查PDA是否合法

检查PDA是否合法接口

* URL：`v1/mobile/device/check`
* METHOD：`POST`
* 请求内容：

    ```
    {
      deviceID: '',      // device id
      macAddress: '',    // MAC地址
      imei : ''    	   // PDA的IMEI号
    }
    ```

* 返回内容：

    ```
    {
        code: 0, // 0: 合法, 其它值: 不合法
        statusCode: 200, // http请求返回值
        message: '操作描述', // 不合法原因
    }
    ```


## 获取所有站点

获取所有站点接口

* URL: `v1/mobile/stations`
* METHOD: `GET`

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
         {
           stationCode: '123', // 站点编号
           stationName: '上海三高' // 站点名称
         }
         {
           stationCode: '234', // 站点编号
           stationName: '上海三高' // 站点名称
         }
         ...
      ]
   }
   ```

## 获取用户可登陆的站点

获取用户可登陆的站点

* URL：`v1/mobile/stations/account/{account}`
* METHOD：`POST`
* 参数:
    * account: 用户账号

* 返回内容：

    ```
    {
        code: 0,
        statusCode: 200,
        message: '操作描述',
        data: [
           {
             stationCode: '123', // 站点编号
             stationName: '上海三高' // 站点名称
           }
           {
             stationCode: '123', // 站点编号
             stationName: '上海三高' // 站点名称
           }
           ...
        ]
    }
    ```

## 登录接口

登录

* URL: `v1/mobile/login`
* METHOD: `POST`

* 请求内容:
    ```
    {
      account: '登录账号',
      pwd: '密码',
      stationCode: '123'
    }
    ```

* 回复内容：

   ```
   {
      code: 0, // 0: 成功, 其它值: 失败
      statusCode: 200,
      message: '操作描述', // 失败原因
   }
   ```

## 获取用户信息接口

获取用户信息接口

* URL: `v1/mobile/account/{account}`
* METHOD: `GET`

* 参数:
    * account: 用户账号

* 回复内容：

   ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
         "userName": "sample string 2",
         "sex": 1,
         "account": "sample string 3",
         "address": "sample string 4",
         "mobile": "sample string 5",
         "tel": "sample string 6"
      }
   }
   ```

## 获取安检信息总数量

获取获取安检信息总数量接口。

* URL: `v1/mobile/inspections/count?account={account}&stationCode={stationCode}`
* METHOD: `GET`

* 参数:
   * account: 用户账号
   * stationCode: 站点编号

* 返回内容：
    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: {
        count: 1000 
      }
    }
    ```


## 获取安检信息

分段获取安检信息接口。

* URL: `v1/mobile/inspections/list?account={account}&stationCode={stationCode}&since={since}&count={count}`
* METHOD: `GET`

* 参数:
   * account: 用户账号
   * stationCode: 站点编号
   * since: 可选，默认为：1，开始索引
   * count: 可选，默认为：1000，记录数

* 返回内容：
    ```
    {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
          {
              id: 123456, // int, 后台数据库表主键值, 唯一
              year: 2016, // int, 年份
              month: 10, // int, 月份
              inspectionId: '安检编号', // string, 唯一
              customerId: '用户编号', // string
              customerName: '用户名称', // string
              customerAddress: '用户地址', // string
              contact: '联系人', // string
              telephone: '联系电话', // string
              type: 1, // int, 安检类型
              dispatchTime: 派工日期, // int, utc时间
              dispatcher: '派工人', // string
              bookId: '统册号', // string
              stationCode: '站点编号', // string, 
              bookSortIndex: 册内序号, // int
              state: 状态, // int
              cardId: '表号', // string
              extend: 'json string' // 扩展信息, 可为空
          },
          ......
      ] 
    }
    ```

	
## 安检回填

安检回填录入保存接口。

* URL: `v1/mobile/inspections/reply?account={account}&stationCode={stationCode}`
* METHOD: `POST`
* 参数:
   * account: 用户账号
   * stationCode: 站点编号

* 请求内容:
    ```
    {
     [
      {
        inspectionId: '安检编号', // string

        insepector: '安检联系人', // string
        telephone: '联系电话', // string
        inspectTime: '安检日期', // utc
        lockNumber: '门锁编号' // string
        logitude: '经度' // double
        latitude: '纬度' // double
        floorheat: '地暖' // boolean
        inspectExponent: 40, // int, 安检指数采集

        doorLock: true, // 门锁, boolean, true: 门锁, false: 门开
        inpection: true, // 安检, boolean, true: 安检，false: 非安检
        rectification: true, // 整改, boolean, true: 整改, false: 非整改

        inspectContent:[//安检内容
                         {
                          "group": 1,
                          "child": 8,
                          "isNormal": false,
                          "reforms": "31,32"
                         },

                         {
                          "group": 1,
                          "child": 8,
                          "isNormal": false,
                          "reforms": "31,32"
                          }
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
		  inspectionId: '安检编号', // string
          extend: 'json string' // 扩展信息, 可为空
		},
        {
          isSuccess: 'false',
          message: '失败原因',
		  inspectionId: '安检编号', // string
          extend: 'json string' // 扩展信息, 可为空
		},
        ...
      }
    }
    ```

## 上传多个文件

上传多个文件。

* URL: `v1/mobile/files/upload?account={account}&stationCode={stationCode}`
* METHOD: `POST`
* 参数:
   * account: 用户账号
   * stationCode: 站点编号

* 请求内容：

  文件内容, byte数组，支持多个文件同时上传，同时上传文件名
  
  ```
  {
    [
      {
        inspectionId: '安检编号1', // string
        fileName: '文件名', // string, 如: 123.jpg, 234.3gp
        字节流, // 文件数据
      }
      {
        inspectionId: '安检编号2', // string
        fileName: '文件名', // string, 如: 123.jpg, 234.3gp
        字节流, // 文件数据
      }
      ...
    ]
  }
  ```

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

![demo](http://128.1.3.186:8095/Update/hotline/shanghai/20160928141931.png)

![demo](http://128.1.3.186:8095/Update/hotline/shanghai/20160928143321.png)


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

## 呼吸包

* URL: `v1/mobile/keepalive`
* METHOD: `POST`

* 请求内容

    ```
    {
      account: '用户账号',         // 用户编号
      deviceId: '123123',  // 设备Id
      imei: 'IMEI号',
      location: {
        locationType： 'wgs84', // 坐标类型
        longitude： 121.3, // 经度
        latitude: 31.5, // 纬度
        radius: 0.0, // 精度
        altitude: 0.0, // 高度
        direction: 0.0, // 方向
        speed: 0.0, // 速度
        time: 1431619200000 // 采集时间
      } // 位置信息
    }
    ```

* 回复内容:

    ```
   {
      code: 0,
      statusCode: 200,
      message: '操作描述',
      data: [
       {               
         time: 123456, // int, utc时间（服务器时间）
       },
       ...
     ]
   }
   ```
