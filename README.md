# KMS Local Activation Tool  
本工具只可用来做个人系统证书环境调试用，一切与之有关的商业活动都是非法的。请支持正版软件。

#### support win7/8.1/10/server2022 office2010~office2021

![image](https://github.com/laomms/KmsTool/blob/main/kms.JPG)     
![image](https://github.com/laomms/KmsTool/blob/main/kms2.png)   

```c
//get HWID
 [UnmanagedFunctionPointer(CallingConvention.StdCall, CharSet=CharSet.Unicode)]
 private delegate int HwidGetCurrentEx(ref IntPtr HWID_Struct, ref uint HWIDsize, ref IntPtr ppbValue, uint dwFlagsAndAttributes); 
 
 //get UpgradeProductKey and pfn
 [UnmanagedFunctionPointer(CallingConvention.StdCall, CharSet=CharSet.Unicode)]
private delegate int SkuGetUpgradeProductKeyEx(int EditionId, string Partner, int ChannelEnum, ref uint dwFlagsAndAttributes, ref IntPtr pPfn); 

 //get GenuineTicket
 [UnmanagedFunctionPointer(CallingConvention.ThisCall, CharSet=CharSet.Unicode)]
 private delegate int CreateGenuineTicketClient(string SessionId, ref uint dwDataSize, ref IntPtr pbData);
 
 
```
##### 软件工作原理:  
KMS本地激活:  
KMS激活逃不过KMS连接代理:SppExtComObj.Exe这个软件,这个工具在系统目录,验证KMS和合法性全部通过这个,只要对这个可执行文件中的RpcStringBindingComposeW和RpcBindingFromStringBindingW进行拦截,让他将KMS服务器指向本地就可以,这样可以在注册表中搭建一个KMS服务器,实现本地激活(早期Windows版本没有这个工具,直接在注册版模拟服务器就可以实现.).

数字激活:  
通过调用系统自带Clipup.exe(或者镜像中的gatherosstate.exe)中的HwidGetCurrentEx获取本机唯一硬件ID,结合系统的相应版本的PFN生成SessionId,然后通过GetGenuineTicket函数生成数字证书.通过安装密钥向微软服务器获取用户证书,跟安装的数字证书进行对比验证激活.

SLIC注入激活:  
通过GetVolumePathNamesForVolumeNameW打开启动分区,加入某品牌的SLIC表信息,然后安装相应的OEM:SLIC密钥,通过对比SLIC表中的公钥来验证激活.支持MBR和GPT分区格式.

密钥激活:  
通过SLInstallProofOfPurchase安装密钥,通过SLGetPKeyInformation获取密钥的SKUID,通过SLActivateProduct激活系统.


















