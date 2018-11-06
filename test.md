<h1> Proxy接口描述文档 </h1>

* [key-value](#0)
  * [读](#0)
    * [getKV](#0)
    * [getKVBatch](#1)
    * [checkKey](#2)
    * [getAllKeys](#3)
  * [写](#4)
    * [setKV](#4)
    * [insertKV](#5)
    * [updateKV](#6)
    * [eraseKV](#7)
    * [delKV](#8)
* [k-k-row](#9)
  * [八、动态更改DCache接口的调用超时时间](#8)
  * [九、禁止遍历获取所有qq号](#9)


<h1 id = "0" > getKV </h1>

```C++
int getKV(const GetKVReq &req, GetKVRsp &rsp)
```
**功能：** 根据key获取value和其他信息

**参数：**  

```C++
struct GetKVReq  
{  
  1 require string moduleName;  //模块名  
  2 require string keyItem;  //键  
  3 require string idcSpecified = ""; //idc区域  
};

struct GetKVRsp  
{  
  1 require string value;  //值  
  2 require byte ver;  //数据版本号  
  3 require int expireTime = 0;  //过期时间  
}; 
```

**返回值**：

返回值 | 含义
------------------ | ----------------
 ET_MODULE_NAME_INVALID                  | 模块名错误
 ET_KEY_AREA_ERR| 当前key不属于本机服务，需要更新路由表重新访问
 ET_DB_ERR     | 数据库读取错误
 ET_KEY_INVALID| key无效
 ET_INPUT_PARAM_ERROR | 参数错误，例如key为空
 ET_SYS_ERR    | 系统异常
 ET_NO_DATA                  | 数据不存在
 ET_SUCC   | 读取数据成功
 
 
 
   
<h1 id = 1 > getKVBatch </h1>
 
```C++
int getKVBatch(const GetKVBatchReq &req, GetKVBatchRsp &rsp)
```

**功能：** 批量查询数据


**参数：**  

```C++
struct GetKVBatchReq
{
  1 require string moduleName; //模块名
  2 require vector<string> keys; //键集合
  3 require string idcSpecified = ""; //idc区域 
};

struct GetKVBatchRsp
{
  1 require vector<SKeyValue> values;  //结果集合
};
```
\geq
**返回值**：

返回值 | 含义
------------------ | ----------------
 ET_MODULE_NAME_INVALID                  | 模块名错误
 ET_KEY_AREA_ERR| 当前key不属于本机服务，需要更新路由表重新访问
 ET_CACHE_ERR| cache读取错误
 ET_INPUT_PARAM_ERROR | 参数错误，例如key数量超过限制或者某个key为空等
 ET_SYS_ERR    | 系统异常
 ET_SUCC   | 批量读取成功
