# oak  
## HDMI_PROCESS.C　　

* MO_CAR_INFO                                  moCommon/moTypes.h                       /opt/moak/include/moSystemBase/
* MO_DRIVE_SEND_DATA                           mo_vision_adas_type.h                    /opt/moak/include/ADASIntegration/ADAS  
* MO_DRIVE_HDMI_DATA                            HDMIType.h                              PERIPHERAL/src/ADAS_HDMI/include/ADAS_HDMI/impl  
* MO_IMG_OBJECT_LIST / MO_IMG_OBJECT / MO_REAL_OBJECT / MO_REAL_OBJECT_LIST      mo_vision_adas_object_type.h   ／opt/moak/include/ADASINtegration/ADAS    


* HDMIFunction.c  
   432 连续４个MO_IMG_OBJECT　　　　　　　　　　　　　 ImgObject[1-4]，　frontObject; 理论计算的一个fake object; 左右两侧的框(现在是0xFF不显示)
   485　pCtrlData->FrontObjectIdx == 1  　　　　　 种类（危险物体）; 1. front. 2. fake
   602　Params.nVanishingPointX  　　　　　　　　　 可行驶区域，车道线相交于远处一个点．
   121 DrawFCWObject()  　　　　 画目标框，　usigned short类型，　坐标，　绘制状态．　需要知道，这些框的优先级别．　　
　　
    33/34行中，memset(pImageData, 0, (nWidth >> MO_MASK_PIXEL_SHIFT)*nHeight*sizeof(char))  这里的偏移如何处理？？？
    1428行中，　函数要在.h中声明

* HDMI_process.c  
    142行中的判断条件
    149行: 　拷贝数据量不再是width*height  - >    pHandle->nMemLen
    214行：　分配内存空间  



*  unsigned shot 大端，小端存储问题　　
*  存入的补码问题
* HDMI_process.c  
    144行中　HDMI_FAST_MEM宏　定义在CMakeLists中，编译的区别

*  lseek(int filde, off_t offset, int whence) 
　　修改文件的起始读写位置 
    offset: 偏移量;
    whence: SEEK_SET 将读写位置指向文件头后再增加offset个位移量　　

*  mmap(void* start, size_t length, int prot, int flags, in fd, off_t offset)
   返回：　成功时，返回被映射区域的指针．
   start：　映射区开始地址，　０系统决定．
   length: 
   prot: 内存保护标志　
   flags:
   fd: 文件描述符
   offset:　被映射对象内容的起点




* 因为C语言并没有规定int占多少字节，为了让代码在多个平台上运行，使用＿u32告诉别人，这个变量占４个字节．　　
* mmap返回１.真实的映射地址. 2. 错误;　　
* pHandle->pShmHead[0]中存储的是两块儿地址HDMI_IMG_DATA_DDR_BASE_0, HDMI_IMG_DATA_DDR_BASE_1  
* 139行得到pHDMI->pMaskData的数值． 149行将pMaskData中的值拷贝到对应的槽里．　　
* pMaskData中的值由　99行中函数moDrawResultMask()来获取．  
