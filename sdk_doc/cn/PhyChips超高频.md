# PhyChips超高频



* [1.PhyChips超高频开发包说明](#PhyChips超高频开发包说明)
* [2.二次开发说明](#二次开发说明)
  * [2.1 AndroidStudio工程配置说明](#AndroidStudio工程配置说明)
  * [2.2 AndroidManifest.xml配置说明](#AndroidManifest配置说明)
  * [2.3 接口说明](#接口说明)
  * [2.4 接口调用流程](#接口调用流程)
  * [2.5 接口调用案例](#接口调用案例)
* [3.二次开发问题汇总](#二次开发问题汇总)

## PhyChips超高频开发包说明

 1.1 支持B类、C类等常用标签;
 
 1.2 PhyChips超高频开发包兼容机器请查看: [开发包兼容机器说明](https://github.com/CoreWise/CWDemo#user-content-zh)

 1.3 [PhyChips超高频开发包下载地址](https://github.com/CoreWise/CWDemo#user-content-zh)
 
 1.4 PhyChips超高频需要依赖串口开发包
 
## 二次开发说明

### AndroidStudio工程配置说明

- 1.添加开发包aar到项目libs目录下

- 2.配置Moudle的build.gradle,参考如下:


```
...
 //2.必须2
 repositories {
     flatDir {
         dirs 'libs'   // aar目录
     }
 }

 dependencies {
     ...
    //串口开发包
    //SerialPort SDK
    compile(name: 'serialport_sdk_20190429', ext: 'aar')
    
    //PhyChips超高频开发包,需要依赖串口开发包
    //PhyChips UHF SDK,need SerialPort SDK
    compile(name: 'phychips_uhf_sdk_20190520', ext: 'aar')
 }
```

### AndroidManifest配置说明

```
<!--PhyChips超高频权限-->


```

### 接口说明

**HX超高频: UHFHXAPI**

主要方法，实现超高频功能必须使用

| 接口名称             | 描述                                            |
| -------------------- | ----------------------------------------------- |
| UHFHXAPI             | *初始化api                                      |
| openHXUHFSerialPort  | *打开串口                                       |
| closeHXUHFSerialPort | *关闭串口                                       |
| open                 | *开启超高频模块                                 |
| close                | *关闭超高频模块                                 |
| startAutoRead2A      | *在清单轮次期间启动自动标签读取操作，标签ID通过通知包发送回用户 |
| startAutoRead2C      | *盘点标签，读到标签就停，并且返回需要读到的数据 |
| readTypeCTagData     | *读取C类标签数据                                |
| writeTypeCTagData    | *写入C类标签数据                                |
| arguments            | *读取、写入标签入参（必须）                     |
| setRegion            | *设置当前区域                                   |

---

备用方法，根据需求使用

| 接口名称                   | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| startAutoRead2             | 启动自动标签读取操作，标签ID通过通知包发送回用户             |
| stopAutoRead2              | 停止自动read2操作                                            |
| readEPC                    | 读取EPC区域                                                  |
| readTID                    | 读取TID区域                                                  |
| stopAutoRead               | 停止自动标签读取操作                                         |
| getReaderInformation       | 从阅读器获取基本信息                                         |
| getRegion                  | 获取当前区域                                                 |
| setSystemReset             | 设置系统级复位                                               |
| getTypeCAISelectParameters | 获取18000-6C空中接口协议命令'选择'参数                       |
| setTypeCAISelectParameters | 设置18000-6C空中接口协议命令'选择'参数                       |
| getTypeCAIQueryParameters  | 获取18000-6C空中接口协议命令'查询'参数                       |
| setTypeCAIQueryParameters  | 设置18000-6C空中接口协议命令'查询'参数                       |
| getCurrentRFChannel        | 获取射频频道，该命令仅对非FH模式有效                         |
| setCurrentRFChannel        | 设置射频通道， 该命令仅对非FHSS模式有效                      |
| getFHAndLBTParameters      | 获取FH和LBT控制                                              |
| setFHAndLBTParameters      | 设置FH和LBT参数                                              |
| getTxPowerLevel            | 获取当前Tx功率水平                                           |
| setTxPowerLevel            | 设置当前Tx功率电平                                           |
| RF_CW_SignalControl        | 打开/关闭连续波（CW）信号， 该命令包仅对空闲模式有效         |
| readTypeCUII               | 读取EPC块（PC + EPC）                                        |
| getFrequencyHoppingTable   | 获取当前跳频表                                               |
| setFrequencyHoppingTable   | 设置当前跳频表                                               |
| getModulationMode          | 获取当前调制模式。 调制模式是Rx调制类型和BLF的组合           |
| setModulationMode          | 获取当前调制模式。 调制模式是Rx调制类型和BLF的组合           |
| getAntiCollisionMode       | 获取防冲突算法                                               |
| setAntiCollisionMode       | 设置防冲突算法                                               |
| blockWriteTypeCTagData     | Blockwrite类型C标签数据                                      |
| blockEraseTypeCTagData     | 块擦除C类标签数据                                            |
| blockPermalockTypeCTag     | BlockPermalock C型标签                                       |
| killTypeCTag               | 删除标签                                                     |
| lockTypeCTag               | 锁定标签中指示的存储库                                       |
| getTemperature             | 获取当前温度                                                 |
| getRSSI                    | 获取RSSI级别                                                 |
| scanRSSI                   | 扫描所有通道的RSSI级别                                       |
| updateRegistry             | 设置注册表更新功能                                           |
| eraseRegistry              | 设置注册表擦除功能                                           |
| getRegistryItem            | 获取注册表项                                                 |


**具体说明:**

- openHXUHFSerialPort

  打开超高频串口模块，建议在onResume中实现
  
- closeHXUHFSerialPort

  关闭超高频串口模块，建议在onPause中实现，与 openHXUHFSerialPort 对应

- open

  开启超高频模块

- close

  关闭超高频模块,与open对应

- startAutoRead2A(AutoRead autoRead)

  开始盘点，在清单轮次期间启动自动标签读取操作，标签ID通过通知包发送回用户
  
  * @param autoRead 收到数据回调监听

- startAutoRead2C(int times, int code, String pwd,
  int sa, int dl, SearchAndRead Interface)
  
  盘点标签，读到标签就停，并且返回需要读到的数据
  
  * @param times 多少秒无数据停止
  * @param code  需要读的区域 0:读取EPC,1:读取TID
  * @param pwd   标签访问密码
  * @param sa    偏移长度
  * @param dl    要读的长度
  * @param Interface 收到数据回调监听

- readTypeCTagData(byte[] arguments)

  读取标签,传参建议使用方法 arguments 赋值
  
  * @param arguments 发送的指令
  * @return 读到的数据

- writeTypeCTagData(byte[] arguments)

  写入标签，传参建议使用方法 arguments 赋值
  
  * @param arguments 发送的指令
  
- arguments(String pwd, short epcLength, String epc, byte mb,int sa, int dl)

  发送的指令  
  
  * @param pwd 标签访问密码
  * @param epcLength  标签id长度
  * @param epc 标签id
  * @param mb  需要读、写的区域 0:EPC,1:TID,2:User
  * @param sa  偏移长度
  * @param dl  要读的长度
  
- setRegion(int argument)

  设置当前区域。
  
   * - Korea (0x11)<br>
   * - US (0x21)<br>
   * - US2 (0x22)<br>
   * - Europe (0x31)<br>
   * - Japan (0x41)<br>
   * - China1 (0x51)<br>
   * - China2 (0x52)<br>

**监听回调接口说明:**

```
//开始盘点
 api.startAutoRead2A(new UHFHXAPI.AutoRead() {

                @Override
                public void timeout() {
                    //超时
                }


                @Override
                public void start() {
                    //开始盘点
                }


                @Override
                public void processing(byte[] data) {
                    //获取数据
                    //标签id
                    String epc = DataUtils.toHexString(data).substring(4);
                }

                @Override
                public void end() {
                    //结束,根据需求重开线程
                }

            });

```

### 接口调用流程

![HXUHF.png](https://i.loli.net/2019/05/08/5cd24de943c73.png)


### 接口调用案例

请按步骤实现方法。

```
    protected ExecutorService pool;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.hxuhf_activity);
        
        api = new UHFHXAPI();//步骤:1
        
        //超高频可控开关，可按需求默认打开
        view.onClick( .. api.open();//步骤:3)
        
        //点击盘点操作,可按需求换成其他操作
        view.onClick( .. pool.execute(task);//步骤:4)
    }

    @Override
    protected void onResume() {
        super.onResume();
        pool = Executors.newSingleThreadExecutor();//步骤:2
        api.openHXUHFSerialPort();
    }

    @Override
    protected void onPause() {
        api.close();//步骤:5
        api.closeHXUHFSerialPort();//步骤:6
        pool.shutdown();
        pool = null;
        super.onPause();
    }
    
    //循环盘点操作
     private Runnable task = new Runnable() {
    
            @Override
            public void run() {
    
                api.startAutoRead2A(new UHFHXAPI.AutoRead() {
    
                    @Override
                    public void timeout() {
                        Log.i("zzdstartAutoRead", "timeout");
                    }
    
    
                    @Override
                    public void start() {
                        //load = soundPool.load(getApplicationContext(), R.raw.ok, 1);
                        Log.i("zzdstartAutoRead", "start");
                        startTime = System.currentTimeMillis();
                    }
    
    
                    @Override
                    public void processing(byte[] data) {
                        String epc = DataUtils.toHexString(data).substring(4);
                        long l = System.currentTimeMillis() - startTime;
                        readTime.put(epc, l);
                        hMsg.obtainMessage(MSG_SHOW_EPC_INFO, epc).sendToTarget();
                        Log.i("zzdstartAutoRead", "data=" + epc + "    time=" + l);
                    }
    
                    @Override
                    public void end() {
                        Log.i("zzdstartAutoRead", "end");
                        Log.i("zzdstartAutoRead", "isStop=" + isStop);
                        Log.e("zzdstartAutoRead", "===================================================================================");
                        if (!isStop) {
                            pool.execute(task);
                        } else {
                            hMsg.sendEmptyMessage(INVENTORY_OVER);
                        }
                    }
    
                });
            }
        };

```

#### 二次开发问题汇总


**1、问：demo标签读取为什么读不了？**

答：需要先开启超高频模块，搜索到标签，**再选择标签**，标签读取显示出标签id说明获取成功。
选择需要读取区域、访问密码（默认 0000 0000），偏移地址，**数据长度**,点击读取。***标签写入同理***

**2、问：demo写入标签为什么一直显示输入长度不对？**

答：写入长度需要先选择数据长度，例如：长度为2，就需要输入 0001 0011，**1长度 = 4个16位数字**，当前demo只支持16位输入
