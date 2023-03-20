# **1.MT2231_sym2_vmx 带签名编译方法**

## 1.工程打开目录:  

`F:\LocalProject\montage\MT2231_sym2_vmx\MICO\config_prj\dvb\symphony\rosemary\demo\rosemary_demo_hd_8m128m\load_rosemary_demo_hd_8m128m_Zuku_dvbs_network.bat`

## 2.编译文件生成目录:  

`F:\LocalProject\montage\MT2231_sym2_vmx\MICO\config_prj\dvb\symphony\rosemary\demo\rosemary_demo_hd_8m128m\rosemary_demo_hd_8m128m\release\binary\symphony.bin`

## 3.将第二步中的`symphony.bin`复制到`A:\LocalProject\MT2231_sym2_vmx\sign_tools\Sym_security_tools\flash_merge_iptv\app_to_be_signed`.

## 4.运行`A:\LocalProject\MT2231_sym2_vmx\sign_tools\Sym_security_tools\flash_merge_iptv\merge_flash.sh`生成`symphony_last.img`.

## 5.将第四步生成的`symphony_last.img`拷贝到`A:\LocalProject\MT2231_sym2_vmx\sign_tools\Sym_security_tools\flash_merge_iptv\usb_upgrade`.

## 6.检查`FileDescriptor.xml`中的`<manufacturerid> 0x84666669 </manufacturerid>`是否与`stbinfo.c`中的`#define    SYS_VMX_OUI          0x84666669`一致.

## 7.运行`A:\LocalProject\MT2231_sym2_vmx\sign_tools\Sym_security_tools\flash_merge_iptv\usb_upgrade\upgrade_file_merge.exe`生成`usb_upgrade.bin`.

## 8.将第七步生成的`usb_upgrade.bin`拷贝至U盘，然后插入STB选择USB升级.

# **2.MT2231_sym2_vmx创建SourceInsight工程需要添加的目录**

## 1.F:\LocalProject\montage\MT2231_sym2_vmx\MICO\inc

## 2.F:\LocalProject\montage\MT2231_sym2_vmx\MICO\prj\dvb\rosemary_demo\includes_zuku_dvbs_network

## 3.F:\LocalProject\montage\MT2231_sym2_vmx\MICO\prj\dvb\rosemary_demo\Zuku_dvbs_network

## 4.替换编译工程配置中的库文件`F:\LocalProject\montage\MT2231_sym2_vmx\MICO\config_prj\dvb\symphony\rosemary\demo\rosemary_demo_hd_8m128m\rosemary_demo_hd_8m128m\rosemary_demo_hd_8m128m_Zuku_dvbs_network.cbp`此文件中的`<Add library="..\..\..\..\..\..\..\lib\lib_symphony\mips4k_lib\libDVBClientProdBC_IPTV.a" />`中的`libDVBClientProdBC_IPTV.a`为生产所用库文件，与之对应的为：

![1678946637868](C:\Users\ccocc\Documents\WeChat Files\wxid_pfr3410alsi422\FileStorage\Temp\1678946637868.png)

>  必要时请替换此库文件！

# **3.日志打开输出方法**

`F:\LocalProject\montage\MT2231_sym2_vmx\MICO\prj\dvb\rosemary_demo\Zuku_dvbs_network\pnd_platform.c`中的

```c
#ifndef VMX_OSD_MICO_SUPPORT_TEST
	mtos_close_printk();
#endif
```

变更为

```c
#ifndef VMX_OSD_MICO_SUPPORT_TEST
 // mtos_close_printk();
#endif
```

即为打开日志输出

# **4.推流工具使用方法（播放节目 + SI表推送）**

![1678934114197](C:\Users\ccocc\Documents\WeChat Files\wxid_pfr3410alsi422\FileStorage\Temp\1678934114197.png)

> 1.注意：此工具不支持win11,仅支持win7,8,10
>
> 2.SI表推送是为了可以搜索到节目，播放节目则是为了有输出，所以最少开两个

## 1.首先需要保证STB盒子和本地电脑在同一局域网下。（路由器实现）

## 2.Interface地址处填写本地IP地址

## 3.port处填写1024+频道号，例如：频道号为1，则port处填写1025

## 4.IP地址处填写235.01.0.1（最后一位为频道号）

> 若推送SI表，则后两位计算公式为：(1024+频道号) / 256      (1024+频道号) %256 

## 5.勾选bitrate选项，通过调整后面的数字控制推送速率

## 6.open准备好的码流文件，然后点击send即可开始推送播放节目

# **5.部分常用接口说明**

## 1.打开/关闭USB日志开关

`MP_VOID MP_HDI_CHIP_Open_Print_To_USB(MP_VOID);`        

`MP_VOID MP_HDI_CHIP_Close_Print_To_USB(MP_VOID);`

```c
MP_VOID MP_HDI_CHIP_Open_Print_To_USB(MP_VOID)
{
	MP_INT32 ret = 0;
	MP_UINT8 filename[128];

	mp_memset(filename, 0, sizeof(filename));

	mt_dbg_file_printf_enable(100*1024, MP_TASK_PRIORITY_PRINT_TO_USB, 1000);

	HDI_Print("usb file printf enable\n");

	mp_strcpy((char*)filename, "usb_dump_log");

	ret = mt_dbg_file_printf_start(filename, MT_DBG_FLAG_START_OVERWR);
	HDI_Print("usb file printf start\n");
	if(ret != SUCCESS)
	{
		HDI_Print("please plug in usb and init fs\n");
		mt_dbg_file_printf_disable();
		HDI_Print("usb file printf disable\n");
		return;
	}

	mt_dbg_serial_force_print(TRUE);
}
```

```c
MP_VOID MP_HDI_CHIP_Close_Print_To_USB(MP_VOID)
{
	MP_INT32 ret = 0;
	ret = mt_dbg_file_printf_stop(0);
	if(ret != SUCCESS)
	{
		HDI_Print("stop usb file printf failed\n");
		return;
	}

	ret = mt_dbg_file_printf_disable();
	if(ret != SUCCESS)
	{
		HDI_Print("disable usb file printf failed\n");
		return;
	}

	HDI_Print("usb file printf disable ok\n");
}
```

