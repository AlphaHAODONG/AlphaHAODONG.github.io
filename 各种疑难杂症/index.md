# 各种疑难杂症


一、如何彻底删除VM ware虚拟机

​		***很显然，对于这类瘤氓软件来说，在控制面板里常规删除已经不能满足它的癖好了，在这里我要对csdn上的一些牛马表示讽刺，他们的一些言论严重误导了真正遇到问题的小白，在这里就不说过多的脏话了，依郭某所见，他们只配在阴暗的角落里扭曲爬行，他们每呼吸一口空气便是对自然的污染，每活着的一秒都是对生命的侮辱***！！！

我们先打开服务，找到VM开头的，把它们全部停止（这是为了防止在之后的删除的时候，会报错此文件正在被其他文件所占用）。

![image-20231213223845310](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336980.png)

然后再打开C:\Program Files (x86)\VMware，看看是不是为空文件，如果不是，把它们全部删除

![image-20210903163817128](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336012.png)

退出到VMware这层目录, 右键删除"VMware"文件夹，删除完成:

![image-20210903163928804](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336038.png)

然后使用强力清理软件，https://www.alipan.com/s/cQYUvKeVa6z，https://www.alipan.com/s/p5R79Spm2ZA，https://www.alipan.com/s/5qyi5ncm4H9，https://www.alipan.com/s/wkwF87uYacq，每个都打开清除一遍

卸载Vmware之后, 一定一定要清理Vmware的注册表信息 ;

按住Windows + R , 在弹出框中输入 "regedit" 调出注册表



![image-20210903164158266](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336978.png)

打开“HKEY_CURRENT_USER”文件夹，找到“Software”文件夹并打开

![image-20210903164444384](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336969.png)

找到“VMware.Inc”，右键删除

![image-20210903164522610](https://alphaguo-1322521250.cos.ap-beijing.myqcloud.com/202312312336989.png)
