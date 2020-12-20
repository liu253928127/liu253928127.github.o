---
title: Linux学习（一）：CentOS-6.8安装与配置
date: 2019-01-20 15:51:40
tags: Linux
---
> **Linux** 是一种[自由和开放源代码](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E5%8F%8A%E5%BC%80%E6%94%BE%E6%BA%90%E4%BB%A3%E7%A0%81%E8%BD%AF%E4%BB%B6)的[类UNIX](https://zh.wikipedia.org/wiki/%E7%B1%BBUnix%E7%B3%BB%E7%BB%9F)[操作系统](https://zh.wikipedia.org/wiki/%E4%BD%9C%E6%A5%AD%E7%B3%BB%E7%B5%B1)。该操作系统的[内核](https://zh.wikipedia.org/wiki/%E5%86%85%E6%A0%B8)由[林纳斯·托瓦兹](https://zh.wikipedia.org/wiki/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9)在1991年10月5日首次发布，在加上[用户空间](https://zh.wikipedia.org/wiki/%E4%BD%BF%E7%94%A8%E8%80%85%E7%A9%BA%E9%96%93)的[应用程序](https://zh.wikipedia.org/wiki/%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F)之后，成为Linux操作系统。

为了便于在 `windows` 上进行 `Linux` 学习，参照前辈的教程，在 `windows` 上基于 `VMware` 或 `VirtualBox`安装Linux系统。

### Linux 环境

- VMware Workstation

  **VMware Workstation** 是 `VMware` 公司销售的[商业软件](https://zh.wikipedia.org/wiki/%E5%95%86%E4%B8%9A%E8%BD%AF%E4%BB%B6)产品之一。该工作站软件包含一个用于[英特尔](https://zh.wikipedia.org/wiki/%E8%8B%B1%E7%89%B9%E5%B0%94)[x86](https://zh.wikipedia.org/wiki/X86)兼容计算机的[虚拟机](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E6%9C%BA)套装，其允许用户同时创建和运行多个x86虚拟机。每个虚拟机可以运行其安装的操作系统，如（但不限于）[Windows](https://zh.wikipedia.org/wiki/Microsoft_Windows)、[Linux](https://zh.wikipedia.org/wiki/Linux)、[BSD变生版本](https://zh.wikipedia.org/wiki/BSD)。

- VirtualBox

  **Oracle VirtualBox **是由[德国](https://zh.wikipedia.org/wiki/%E5%BE%B7%E5%9C%8B) `InnoTek` 软件公司出品的[虚拟机](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8)[软件](https://zh.wikipedia.org/wiki/%E8%BB%9F%E9%AB%94)，现在则由[甲骨文公司](https://zh.wikipedia.org/wiki/%E7%94%B2%E9%AA%A8%E6%96%87%E5%85%AC%E5%8F%B8)进行开发，是甲骨文公司xVM[虚拟化](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E5%8C%96)平台技术的一部分。它提供用户在[32位](https://zh.wikipedia.org/wiki/32%E4%BD%8D%E5%85%83)或[64位](https://zh.wikipedia.org/wiki/64%E4%BD%8D%E5%85%83)的[Windows](https://zh.wikipedia.org/wiki/Microsoft_Windows)、[Solaris](https://zh.wikipedia.org/wiki/Solaris)及[Linux](https://zh.wikipedia.org/wiki/Linux) [操作系统](https://zh.wikipedia.org/wiki/%E4%BD%9C%E6%A5%AD%E7%B3%BB%E7%B5%B1)上虚拟其它[x86](https://zh.wikipedia.org/wiki/X86)的操作系统。

为了方便移植，这里选用了 `VMware` 作为 `Linux` 学习的工具。

### Linux 下载

- CentOS 6.8

  从 `CentOS` 的官网获取 `CentOS` 的安装 `ISO` ：http://isoredirect.centos.org/centos/6/isos/x86_64/

### Linux 安装

- 安装环境

  1. `VMware` 版本： `15.0.0 build-10134415`
  2. `Linux` 版本：`CentOS 6.8`

- VMware新建Linux注意事项

  1. 新建虚拟机时选自定义新建虚拟机

     ![2019012002](/images/2019012002.png)

  2. 在安装客户机操作系统的选项中，选择稍后安装操作系统，然后点击下一步

     备注：此处如果直接选择安装程序光盘映像文件 `iso` 的选项，并导入 `CentOS` 系统，`Vmware Workstation` 将会执行简易安装，并且自动配置安装 `CentOS` 时的很多参数，不利于我们自定义系统。因此建议选择为稍后安装操作系统。

     ![2019012004](/images/2019012004.png)

  3. 在选择客户机操作系统的选项中，先将客户操作系统的大类选项中选择为Linux，再将版本中的选项选择为Centos 64位，然后点击下一步

     ![2019012005](/images/2019012005.png)

  4. 在命名虚拟机的选项中，首先为该次创建的虚拟机命名，也可保留为默认，然后在位置选项中可以为该次创建的虚拟机在计算机硬盘中指定对应的存储路径，然后点击下一步

     ![2019012006](/images/2019012006.png)

  5. 在处理器配置的选项中，可以为该次创建的虚拟机分配处理器的数量与每个处理器的核心数量，建议按照物理计算机的实际CPU数量或实际CPU数量的一半的去分配，指定完成后，点击下一步

     ![2019012007](/images/2019012007.png)

  6. 在此虚拟机的内存选项中，可以为该次创建的虚拟机分配可使用的内存大小，鉴于本次安装的是Linux，此处分配2G-4G即可，分配完成后，点击下一步

     ![2019012008](/images/2019012008.png)

  7. 选择网络类型的选项中，选择默认的NAT模式即可

     ![2019012009](/images/2019012009.png)

  8. 在选择磁盘的选项中，选择创建新虚拟磁盘即可，然后点击下一步

     ![2019012012](/images/2019012012.png)

  9. 在指定磁盘容量大小的选项中，首先看到最大磁盘大小这一选项，一般来说，根据建议大小调整大小即可，不要去勾选立即分配所有磁盘空间，当然根据自己的需求与硬盘容量去适当的调整大小也是可以的，然后选择将虚拟磁盘存储为单个文件的选项，最后点击下一步

     ![2019012013](/images/2019012013.png)

  10. 在已准备好创建虚拟机的选项中，根据自己的需要点击自定义硬件，添加、修改或者删除部分需要的硬件。然后点击完成

     ![2019012015](/images/2019012015.png)

  11. 点击编辑虚拟机设置选择相应的iso文件

      备注：此处如果是载入Centos6.8的ISO，需要载入CentOS-6.8-x86_64-bin-DVD1.iso，对于Centos 6.8而言，会存在两个镜像，一个是CentOS-6.8-x86_64-bin-DVD1.iso，另一个是CentOS-6.8-x86_64-bin-DVD2.iso。DVD1中通常是系统和部分软件，DVD2中通常是附加软件包，如有需要，可以在安装完DVD1中的Centos6.8系统后再另行安装DVD2。

      ![2019012016](/images/2019012016.png)

  12. 最后点击开启虚拟机

####  Linux 系统安装注意事项

1. 虚拟机启动后，即会自动开始引导光驱中的 `CetnOS6.8` 的 `ISO` 镜像，在 `CentOS6.8` 的引导界面上选择 `Install system with basic video driver` 即可开始安装进程

   备注：

   - Install or upgrade an existing system（安装或者升级一个已经存在的系统）
   - Install system with basic video driver（以基本的视频驱动程序来安装系统）
   - Rescue installed system（援救已安装的系统）
   - Boot from local drive（从本地驱动器引导）
   - Memory test（内存测试）

   ![2019012017](/images/2019012017.png)

2. 在运行一段自检程序之后，将会进入 `CentOS6.8` 的安装程序，首先看到的是 `Media Test`，选择 `Skip`跳过

   备注：

   - 其作用是为了在安装系统前检测安装介质是否存在问题，会花费较长时间。一般来说，不需要进行检测，选择跳过即可。

   ![2019012018](/images/2019012018.png)

3. 其后会询问在安装进程中需要使用哪种语言，选择简体中文，键盘选美国英文键盘

4. 选择基础存储设备安装

   ![2019012020](/images/2019012020.png)

5. 接上一步选项下一步之后，此处是发现刚才选定的存储设备内可能包含数据，询问是否要忽略这些数据，选择是即可，点击下一步

   备注：

   - 此处因为是指定的空闲硬盘空间进行安装，因此可以确定没有数据，所以可以选择直接放弃所有数据，在实际操作中，还是要确认指定的存储设备是否有重要数据存在
   - 勾选的意思是在不检测所有存储设备内的分区与文件系统的前提下，应用本次忽略所有数据的选项到所有的存储设备，同理，因本次是空间硬盘空间安装，可以勾选本选项

   ![2019012021](/images/2019012021.png)

6. 为本台主机命名一个主机名，以便让本主机可以在网络上通过该主机名被唯一的辨识

7. 选择时区

8. 此处要求设置的是root密码，要求输入两次，两次必须一样，然后点击下一步

   备注：

   - 如果root密码设置的太简单，会提示你的密码太弱小，让你更改，如果要强行使用，点击Use Anyway即可

   ![2019012024](/images/2019012024.png)

9. 设置安装系统的方式

   备注：

   - 使用所有空间：
   - 替换现有 `Linux` 系统
   - 缩小现有系统
   - 使用剩余空间
   - 创建自定义布局

   ![2019012025](/images/2019012025.png)

10. 进入自定义布局页面

    备注：

    - 先创建 `/boot` 分区用来存放系统内核引导文件，一个空间大小为200M，文件系统类型为ext4的分区

      ![2019012027](/images/2019012027.png)

    - `swap`分区（交换分区），充当虚拟内存使用，大小约为物理内存的2倍，如果内存够大，可以不配置`swap`文件系统

      ![2019012028](/images/2019012028.png)

    - `/`分区分配剩余其他空间，因为其他目录都是在 `/`下生成

    ![2019012029](/images/2019012029.png)

![2019012030](/images/2019012030.png)

11. 在上图中分配完自定义布局后，点击下一步，会弹出格式化警告信息（Fromat Warning），告诉使用者该块磁盘上的数据将被摧毁，将按照使用者指定的布局来重新分区。点击格式化即可

12. 此时会再弹出警告框，提示分区设置已选定，即将写入到磁盘中，所有原先在磁盘中的数据都将被删除，是否继续，点击将修改写入磁盘即可

    ![2019012032](/images/2019012032.png)

13. 此时会告诉你系统引导驱动器的位置被安装在何处，如果不想改变位置，可以保持默认，点击下一步继续
14. 到可选软件包安装界面后，这里我们选 `Desktop` 类型安装

![2019012034](/images/2019012034.png)

15. 然后等待进度条加载完成，点击Reboot，即可完成Centos6.8的安装