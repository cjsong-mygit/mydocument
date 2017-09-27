### openstack镜像 ###
官方提供可以使用的镜像有


- CentOS
- CirrOS
- Debin
- Fedora
- Microsoft Windows
- Ubuntu
- openSUSE and SUSE Linux Enterprise Server
- Red Hat Enterprise Linux
### 镜像要求 ###
针对linux镜像，可以安装cloud-init包满足要求。


- 磁盘分区，在启动时重新调整root分区（cloud-init)
- 没有硬编码的MAC地址信息
- SSH服务器运行
- 禁用防火墙
- 使用ssh公共密匙访问实例(cloud-init)
- 处理用户数据和其他元数据(cloud-init)
- Linux内核中的半虚拟化Xen支持(Xen只支持低于3.0的内核版本）
#### 磁盘分区，在启动时重新调整root分区（cloud-init) ####


#### 制作镜像的基本原理 ####
使用过vmware workstation、vsphere的人都知道，创建一个虚拟机后，会生成一个虚拟机文件，我们只需要拷贝这个文件，在另一个环境中打开，效果和之前的虚拟机完全相同；在OpenStack平台上类似，我们先用qemu-img创建一个qcow2格式的虚拟磁盘，再用libvirt的虚拟光驱导入cnetos系统的iso文件，按照安装系统步骤装好系统，进入系统做一些必要的配置修改，关机后，那个qcow2格式的文件就是一个镜像文件了，也就是说，镜像本质上是一块虚拟磁盘。技术层面的封装关系是kvm<qemu<libvirt<openstack，所以其实我们是用libvirt启动一个虚拟机，做成符合条件的镜像，提供给openstack或其他libvirt使用而已，libvirt启动虚拟机的过程可以是纯敲命令，也可以是界面操作。